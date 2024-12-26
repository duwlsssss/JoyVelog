<p>현재 진행하는 사이드 프로젝트에서 <code>supabase auth</code>를 사용해 회원 관리를 하고 있습니다.</p>
<p>계정 해지 기능을 추가하려고 하는데
<a href="https://supabase.com/docs/reference/javascript/auth-admin-deleteuser">공식문서</a>에는 <code>service_role</code> key를 가진 admin이 특정 사용자를 삭제하는 방법만 나와있었습니다.</p>
<p>제가 원하는 건 사용자가 계정 해지를 할 수 있는 것이었습니다.</p>
<br />

<p><strong>DataBase Function을 만들어두고 그걸 호출하는 방식을 사용했습니다.</strong></p>
<br />

<ol>
<li><code>delete_user</code> 라는 함수를 만들어 놓고 </li>
</ol>
<pre><code class="language-sql">create or replace function delete_user()
returns void as $$
begin
  delete from auth.users where id = auth.uid();
end;
$$ language plpgsql security definer;</code></pre>
<br />

<ol start="2">
<li><p>supabase.rpc() 메서드를 호출해 현재 세션의 사용자 정보를 삭제했습니다.</p>
<p> ➕ rpc() 메서드: Remote Procedure Call, 클라이언트에서 데이터베이스에 정의된 PostgreSQL 함수를 호출할 때 사용</p>
</li>
</ol>
<pre><code class="language-js">export const useDeactivateAccount = () =&gt; {
  const { clearUser } = useUserStore()
  const handleError = useErrorHandler()
  const navigate = useNavigate()

  const { mutateAsync: deactivateAccount, isPending } = useMutation({
    mutationFn: async () =&gt; {
      // Supabase 사용자 삭제 - 현재 세션의 사용자 정보 삭제
      const { error } = await supabase.rpc('delete_user') // DB 함수 실행

      if (error) throw error

      // 전역 상태에서 사용자 정보 제거
      clearUser()
      // 성공 시 로그인 페이지로 이동
      navigate('/signin', { replace: true })
    },
    onError: error =&gt; handleError('계정 해지', error)
  })

  return { deactivateAccount, isPending }
}</code></pre>
<br />

<p>참고로 auth.users에서 삭제가 일어나면 public.users에서도 삭제되게 설정해놔야 합니다.</p>
<pre><code class="language-sql">create table public.users(
  id uuid references auth.users on delete cascade,
  ...
);</code></pre>
<br />

<blockquote>
<p>참고자료</p>
</blockquote>
<ul>
<li><a href="https://github.com/orgs/supabase/discussions/1066">https://github.com/orgs/supabase/discussions/1066</a></li>
</ul>