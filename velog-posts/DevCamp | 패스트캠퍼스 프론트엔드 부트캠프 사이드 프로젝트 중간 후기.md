<p>사이드 프로젝트 스터디를 하며 supabase auth를 활용해 유저를 관리하기로 했습니다. </p>
<p>테이블 생성과 auth 부분을 담당해 회원가입, 로그인(이메일, 구글), 프로필 수정을 맡았습니다.</p>
<p>supabase로 회원 관리를 어떻게 했는지 포스팅해보겠습니다.</p>
<p>supabase로 테이블을 작성하고 나면 어떻게 써야 하는지 <code>api docs</code>를 자동으로 생성해줍니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/kimlj0814/post/a96580e3-37e8-4425-8caf-57c61c2c6e1e/image.png" /></p>
<p>다른 팀원들이 편하게 DB와 상호작용할 수 있어 이 부분이 정말 편했어요. 
저도 이 부분을 많이 참고했어요.</p>
<p>이제 auth를 사용하는 방법을 적어볼게요.</p>
<h3 id="1️⃣-authusers와-publicusers-연결">1️⃣ auth.users와 public.users 연결</h3>
<pre><code class="language-sql">create function public.handle_user_update()
returns trigger as $$
declare
  base_nickname text;
  final_nickname text;
  counter int := 0;
begin
  -- 기본 닉네임 설정
  base_nickname := coalesce(
    new.raw_user_meta_data-&gt;&gt;'nickname',
    -- 구글 로그인
    new.raw_user_meta_data-&gt;&gt;'full_name',
    new.raw_user_meta_data-&gt;&gt;'name',
    split_part(new.email, '@', 1)
  );

  -- 초기 닉네임 설정
  final_nickname := base_nickname;

  -- 닉네임 중복 체크 및 숫자 추가
  while exists (select 1 from public.users where nickname = final_nickname) loop
    counter := counter + 1;
    final_nickname := base_nickname || counter::text;
  end loop;

  insert into public.users(user_id, email, nickname, profile_picture_path)
  values (
    new.id,
    new.email,
    final_nickname,
    coalesce(
      new.raw_user_meta_data-&gt;&gt;'profile_picture_path',
      -- 구글 로그인
      new.raw_user_meta_data-&gt;&gt;'avatar_url',
      new.raw_user_meta_data-&gt;&gt;'picture'
    )
  );
  return new;
end;
$$ language plpgsql security definer;

create trigger on_auth_user_updated
  after update on auth.users
  for each row execute procedure public.handle_user_update();</code></pre>
<p>auth.users에 레코드가 삽입되면 그 정보를 기반으로 public.users에도 레코드를 넣어주는 함수와 trigger를 작성합니다.</p>
<br />

<h3 id="2️⃣-회원가입-이메일-로그인-구글-로그인-프로필-수정-훅을-구현해-각-button에-연결하기">2️⃣ 회원가입, 이메일 로그인, 구글 로그인, 프로필 수정 훅을 구현해 각 button에 연결하기</h3>
<pre><code class="language-js">// hooks/mutations/useSignUp.ts
import { useMutation } from '@tanstack/react-query'
import { supabase } from '../../../supabaseConfig'
import { TSignUpFormValues } from '@/schemas/user/signUpSchema'
import { useErrorHandler } from '@/hooks/useErrorHandler'
import { isApiError } from '@/utils/isApiError'

export const useSignUp = (
  onError: (field: keyof TSignUpFormValues, message: string) =&gt; void
) =&gt; {
  const handleError = useErrorHandler()

  const { mutateAsync: signUp, isPending } = useMutation&lt;
    void,
    Error,
    TSignUpFormValues
  &gt;({
    mutationFn: async ({ email, password, nickname }: TSignUpFormValues) =&gt; {
      // Supabase 회원가입
      const { error } = await supabase.auth.signUp({
        email,
        password,
        options: {
          data: {
            nickname
          }
        }
      })

      if (error) throw error // onError에서 처리
    },
    onError: error =&gt; {
      if (isApiError(error) &amp;&amp; error.status &gt;= 400 &amp;&amp; error.status &lt; 500) {
        // 400번재 에러는 폼에 에러 메시지 표시
        onError('email', '이메일을 다시 확인해주세요')
        return
      }
      handleError('회원가입', error)
    }
  })

  return { signUp, isPending }
}



// components/sign-up/SignUpForm.tsx
export const SignUpForm = () =&gt; {
  // ...
  return (
  //...
    &lt;S.SubmitButton
        color="pink"
        disabled={
        isSubmitting ||
        Object.keys(errors).length &gt; 0 ||
        isPending ||
        !validFields.nickname ||
        !validFields.email
    }&gt;
      {isPending ? '가입 중...' : '가입하기'}
    &lt;/S.SubmitButton&gt;
  );</code></pre>
<p>저는 Password-based Auth에서도 email을 활용했는데 이 외에도 Phone number, Social login(Oauth)를 사용 가능해요! (Google, Facebook, Apple...)</p>
<p><a href="https://supabase.com/docs/guides/auth/passwords">supabase auth 문서</a> 참고하시면 됩니다.</p>
<br />

<p>➕ 구글 로그인 훅</p>
<pre><code class="language-js">import { useMutation } from '@tanstack/react-query'
import { supabase } from '../../../supabaseConfig'
import { useErrorHandler } from '@/hooks/useErrorHandler'
import { getURL } from '@/utils/getURL'

export const useGoogleSignIn = () =&gt; {
  const handleError = useErrorHandler()

  const { mutateAsync: googleSignIn, isPending } = useMutation&lt;void, Error&gt;({
    mutationFn: async () =&gt; {
      // Supabase 구글 로그인
      const { error } = await supabase.auth.signInWithOAuth({
        provider: 'google',
        options: {
          redirectTo: `${getURL()}/auth/callback`,
          // 매번 새롭게 로그인하게
          queryParams: {
            access_type: 'offline',
            prompt: 'consent'
          }
        }
      })

      if (error) throw error
    },
    onError: error =&gt; {
      handleError('구글 로그인', error)
    }
  })

  return { googleSignIn, isPending }
}</code></pre>
<p>구글 로그인도 마찬가지로 <a href="https://supabase.com/docs/guides/auth/social-login/auth-google">공식문서</a>를 하나씩 따라가면 됩니다.</p>
<p>사용자는 Google의 동의 화면으로 이동 후, 최종적으로 액세스 및 리프레시 토큰 쌍과 함께 앱으로 리다이렉트돼요.</p>
<pre><code class="language-js">queryParams: {
    access_type: 'offline', // refresh token을 받아서 사용자가 로그아웃해도 앱은 Google API 에 액세스 가능
    prompt: 'consent', // 매번 동의 화면을 보여줌 (refresh token 강제 재발급)
}</code></pre>
<p>구글은 기본적으로 리프레시 토큰을 제공하지 않으므로, provider_refresh_token을 추출하려면 signInWithOAuth()에 이렇게 전달해줘야 해요.</p>
<br />

<p>저는 구글 로그인 중</p>
<pre><code>redirectTo: `${getURL()}/auth/callback` </code></pre><p>이 부분에서 많이 헤맸어요.</p>
<pre><code>redirectTo: getURL() // 앱 주소</code></pre><p>처음엔 이렇게 쓰고 테이블엔 레코드가 추가되는데 리다이렉트가 안되서 눈물이 났는데요... </p>
<p><strong><code>signInWithOAuth</code>를 호출할 때, 콜백 경로를 가리키는 redirectTo URL은 리디렉션 허용 목록(redirect allow list)에 추가되어야 한다고 해요.</strong></p>
<p>저는 기본 앱 주소에 인증되지 않은 사용자는 접근하지 못하게 해 놔서 리다이렉트가 안됐던 거였어요.</p>
<pre><code>signInWithOAuth 호출 
-&gt; 구글 로그인 페이지로 리다이렉트구글 인증 완료 
-&gt; 지정된 redirectTo URL로 리다이렉트 (이 시점에서 세션 초기화 시작) 
-&gt; 세션 설정 완료 </code></pre><p>이 단계로 이뤄지므로 <code>redirectTo URL</code>은 꼭 인증되지 않은 사용자도 접근할 수 있는 주소여야 해요!! </p>
<br />

<h3 id="3️⃣-useauthstatechange로-세션-상태-반영">3️⃣ useAuthStateChange로 세션 상태 반영</h3>
<pre><code class="language-js">import { useEffect, useState, useCallback } from 'react'
import { supabase } from '../../supabaseConfig'
import queryClient from '@/lib/queryClient'
import { Session } from '@supabase/supabase-js'
import { SupabaseUserData, User } from '@/types/auth'
import fetchUserProfile from '@/services/auth/fetchUserProfile'

const useAuthStateChange = () =&gt; {
  const [session, setSession] = useState&lt;Session | null&gt;(null)
  const [user, setUser] = useState&lt;User | null&gt;(() =&gt; {
    // 초기값을 localStorage에서 가져오기
    const storedUSer = localStorage.getItem('user')
    return storedUSer ? JSON.parse(storedUSer) : null
  })

  const updateUser = useCallback(async (session: Session | null) =&gt; {
    if (!session) {
      setUser(null)
      localStorage.removeItem('user')
      return
    }

    try {
      const userData: SupabaseUserData = await queryClient.fetchQuery({
        queryKey: ['userProfile', session.user.id],
        queryFn: () =&gt; fetchUserProfile(session.user.id)
      })
      if ('error' in userData) throw userData.error
      const userInfo = {
        userId: session.user.id,
        email: session.user.email ?? '',
        nickname: userData.nickname ?? '',
        profilePicturePath: userData.profile_picture_path ?? ''
      }

      localStorage.setItem('user', JSON.stringify(userInfo))
      setUser(userInfo)
    } catch (error) {
      console.error('Failed to update user:', error)
    }
  }, [])

  const handleAuthChange = useCallback(
    async (event: string, currSession: Session | null) =&gt; {
      console.log('Event type:', event)
      setSession(() =&gt; currSession)
      switch (event) {
        case 'INITIAL_SESSION':
        case 'SIGNED_IN':
        case 'TOKEN_REFRESHED':
        case 'USER_UPDATED': {
          // 프로필 데이터 새로 가져오기
          if (!currSession?.user) return
          updateUser(currSession)
          break
        }

        case 'SIGNED_OUT': {
          localStorage.removeItem('user')
          updateUser(null)
          queryClient.clear()
          break
        }
      }
    },
    [updateUser]
  )

  useEffect(() =&gt; {
    // 로컬 스토리지에서 세션 복원
    supabase.auth.getSession().then(async ({ data: { session } }) =&gt; {
      setSession(session)
      if (session?.user) {
        updateUser(session)
      }
    })

    // 세션 변경 구독
    const {
      data: { subscription }
    } = supabase.auth.onAuthStateChange(handleAuthChange)

    return () =&gt; subscription.unsubscribe()
  }, [handleAuthChange, updateUser])

  return { session, user }
}

export default useAuthStateChange</code></pre>
<p>마지막으로 useAuthStateChange로 event와 session을 감지해 원하는 처리를 해주면 됩니다.</p>
<br />

<p>구글 로그인에서 공식문서 제대로 안 읽고 시간을 많이 버린 것 같지만,,, 🥹
지치지 말고 남은 기간도 파이팅!!</p>