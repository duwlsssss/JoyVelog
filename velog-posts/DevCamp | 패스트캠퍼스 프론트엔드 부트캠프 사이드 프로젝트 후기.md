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
<p>auth.users에 레코드가 삽입되면 그 정보를 기반으로 public.users에도 레코드를 넣어주는 trigger를 작성합니다.</p>
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

  const { mutateAsync: signUp, isPending } = useMutation({
    mutationFn: async ({ email, password, nickname }: TSignUpFormValues) =&gt; {
      // Supabase 회원가입
      const { data, error } = await supabase.auth.signUp({
        email,
        password,
        options: {
          data: {
            nickname
          }
        }
      })

      if (error) throw error // onError에서 처리

      return data // 성공시 데이터 반환
    },
    onError: error =&gt; {
      if (isApiError(error) &amp;&amp; error.status &gt;= 400 &amp;&amp; error.status &lt; 500) {
        // 400번대 에러는 폼에 에러 메시지 표시
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

  const { mutateAsync: googleSignIn, isPending } = useMutation({
    mutationFn: async () =&gt; {
      // Supabase 구글 로그인
      const { data, error } = await supabase.auth.signInWithOAuth({
        provider: 'google',
        options: {
          redirectTo: `${getURL()}/auth/callback`,
          // 매번 새롭게 로그인하게
          queryParams: {
            access_type: 'offline',
            prompt: 'consent', 
          }
        }
      })

      if (error) throw error

      return data;
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

<p>저는 </p>
<pre><code>redirectTo: `${getURL()}/auth/callback` </code></pre><p>이 부분에서 많이 헤맸어요.</p>
<pre><code>redirectTo: getURL() // 앱 주소</code></pre><p>이렇게 쓰고 테이블엔 레코드가 추가되는데 리다이렉트가 안되서 눈물이 났는데요... </p>
<p><strong><code>signInWithOAuth</code>를 호출할 때, 콜백 경로를 가리키는 redirectTo URL은 리디렉션 허용 목록(redirect allow list)에 추가되어야 한다고 해요.</strong></p>
<p>저는 기본 앱 주소에 인증되지 않은 사용자는 접근하지 못하게 해 놔서 문제가 생겼어요.</p>
<pre><code>signInWithOAuth 호출 
-&gt; 구글 로그인 페이지로 리다이렉트구글 인증 완료 
-&gt; 지정된 redirectTo URL로 리다이렉트 (이 시점에서 세션 초기화 시작) 
-&gt; 세션 설정 완료 </code></pre><p>이 단계로 이뤄지므로 <code>redirectTo URL</code>은 꼭 인증되지 않은 사용자도 접근할 수 있는 주소여야 해요!! </p>
<br />

<h3 id="3️⃣-useauthstatechange로-세션-상태-반영">3️⃣ useAuthStateChange로 세션 상태 반영</h3>
<pre><code class="language-js">import { useEffect } from 'react'
import { useNavigate } from 'react-router-dom'
import { supabase } from '../../supabaseConfig'
import { useUserStore } from '@/stores/userStore'
import { queryClient } from '../App'

// 프로필 쿼리 함수
const fetchUserProfile = async (userId: string) =&gt; {
  const { data } = await supabase
    .from('users')
    .select('email, nickname, profile_picture_path')
    .eq('user_id', userId)
    .single()
  return data
}

export const useAuthStateChange = () =&gt; {
  const navigate = useNavigate()
  const { setUser, clearUser } = useUserStore()

  // 로그아웃 처리 함수
  const handleSignOut = () =&gt; {
    clearUser()
    queryClient.clear()
    navigate('/signin')
  }

  useEffect(() =&gt; {
    // 초기 세션 체크
    supabase.auth.getSession().then(async({ data: { session } }) =&gt; {
      if (!session?.user) return

      const userData = await queryClient.fetchQuery({
        queryKey: ['userProfile', session.user.id],
        queryFn: () =&gt; fetchUserProfile(session.user.id)
      })

      if (userData) {
        setUser({
          email: userData.email,
          nickname: userData.nickname || '',
          profilePicturePath: userData.profile_picture_path || ''
        })
      }
    })

    const {
      data: { subscription }
    } = supabase.auth.onAuthStateChange(async (event, session) =&gt; {
      // console.log('Event type:', event)
      // console.log('Session:', session)

      switch (event) {
        case 'SIGNED_IN':
          if (session?.user) {
            if (!session?.user) return
            const userData = await queryClient.fetchQuery({
              queryKey: ['userProfile', session.user.id],
              queryFn: () =&gt; fetchUserProfile(session.user.id)
            })

            if (userData) {
              setTimeout(() =&gt; {
                setUser({
                  email: userData.email,
                  nickname: userData.nickname || '',
                  profilePicturePath: userData.profile_picture_path || ''
                })
                queryClient.invalidateQueries({ queryKey: ['userProfile'] })
                navigate('/')
              }, 0)
            }
          }
          break

        case 'SIGNED_OUT':
        case 'TOKEN_REFRESHED':
          if (!session) handleSignOut()
          break

        case 'USER_UPDATED':
          if (session?.user) {
            queryClient.invalidateQueries({ 
              queryKey: ['userProfile', session.user.id] // 프로필 쿼리 무효화
            })
          }
          break
      }
    })

    return () =&gt; {
      subscription.unsubscribe()
    }
  }, [])
}</code></pre>
<p>마지막으로 useAuthStateChange로 event와 session을 감지해 원하는 처리를 해주면 됩니다.</p>
<br />

<p>구글 로그인에서 공식문서 제대로 안 읽고 시간을 많이 버린 것 같지만,,, 🥹
지치지 말고 남은 기간도 파이팅!!</p>