<p>토이 프로젝트 3도 열심히 달리고 있는데요!!</p>
<p>프로젝트가 어느 정도 마무리되며 이번 프로젝트의 필수 요구 사항인 <code>playwright, cypress 등을 활용해 코드 기반 테스트에 대한 기술적 이해를 바탕으로 단위 테스트 및 종단 테스트를 활용해 보기</code> 를 구현했습니다.</p>
<p>편리한 디버깅 툴을 제공하는 playwright를 사용해 e2e 테스트를 진행했습니다.</p>
<p>지금 구현된 페이지가 로그인, 회원가입, 프로필 수정이기 때문에 각각에 맞는 코드를 작성해 테스트했습니다.</p>
<p><code>tests</code> 폴더에 <code>테스트할 기능 or 페이지.spec.ts</code> 파일을 작성하고 테스트 코드를 작성했습니다.</p>
<p><code>package.json</code>에는 </p>
<pre><code class="language-json">{
  "scripts": {
    "test": "playwright test",
    "test:ui": "playwright test --ui",
    "test:headed": "playwright test --headed"
  },
}</code></pre>
<p>이렇게 작성해 놨습니다.</p>
<p>아래는 프로필 수정 페이지를 테스트 하는 코드입니다.</p>
<pre><code class="language-js">// tests/editProfile.spec.ts

import { test, expect } from '@playwright/test';

test.describe('프로필 수정 페이지', () =&gt; {
  test.beforeEach(async ({ page }) =&gt; {
    // 로그인 후 진행
    await page.goto('/auth/sign-in');

    await page.fill('input[id="email"]', 'test2@email.com');
    await page.fill('input[id="password"]', '12345678');

    await page.locator('button:nth-of-type(1)').click();

    await expect(page).toHaveURL('/'); // 로그인을 하면 홈으로 이동합니다
    await page.goto('/edit'); // 프로필 수정 페이지로 이동
  });

  test('프로필 정보 수정', async ({ page }) =&gt; {
    // 닉네임 변경
    await page.fill('input[id="nickname"]', '새로운닉네임');
    await page.getByRole('button', { name: '중복 확인' }).click();
    await expect(page.getByText('사용 가능한 닉네임입니다')).toBeVisible();

    // 한줄 소개 변경
    await page.fill('input[id="shortIntro"]', '새로운 한줄 소개입니다.');

    // 비밀번호 변경
    await page.fill('input[id="password"]', '123456');
    await page.fill('input[id="confirmPassword"]', '123456');

    // 저장 버튼 클릭
    await expect(page.getByRole('button', { name: '변경 저장' })).toBeEnabled();
    await page.getByRole('button', { name: '변경 저장' }).click();

    await expect(page).toHaveURL('/');
    await page.goto('/edit');

    await expect(page.locator('input[id="nickname"]')).toHaveValue(
      '새로운닉네임',
    );
    await expect(page.locator('input[id="shortIntro"]')).toHaveValue(
      '새로운 한줄 소개입니다.',
    );
  });

  test('변경 되돌리기', async ({ page }) =&gt; {
    await page.fill('input[id="shortIntro"]', '더 새로운 한줄 소개입니다.');
    await expect(
      page.getByRole('button', { name: '변경 되돌리기' }),
    ).toBeVisible();
    await page.getByRole('button', { name: '변경 되돌리기' }).click();

    // 원래 값으로 복구됐는지 확인
    await expect(page.getByText('한줄소개를 입력해주세요')).toBeVisible();
  });

  test('회원 탈퇴', async ({ page }) =&gt; {
    await page.getByRole('button', { name: '회원 탈퇴' }).click();
    await expect(page.getByText('회원 탈퇴 하시겠습니까?')).toBeVisible();

    // 취소
    await page.getByRole('button', { name: '아니오' }).click();
    await expect(page.getByText('회원 탈퇴 하시겠습니까?')).not.toBeVisible();
  });
});</code></pre>
<p><img alt="테스트 디버깅 ui" src="https://velog.velcdn.com/images/kimlj0814/post/9bca5886-ab48-496b-a00d-d0bf193fd538/image.png" /></p>
<p><code>npm run test:ui</code> 로 디버깅 ui를 띄우고 원하는 테스트를 선택해 실행하면 됩니다.
테스트 코드에도 하이라이팅이 되며 오류가 어디서 발생했는지 찾기 쉬웠어요.</p>
<br />

<p>처음엔 어차피 제가 직접 코드를 짜서 테스트를 하는 것이기 때문에 수동으로 값을 자유롭게 바꿔가며 테스트 하는 게 더 편하다는 느낌이 들었습니다.</p>
<p>그런데 테스트 과정에서 로그인, 회원가입 페이지에서 모든 필드가 안 채워져 있어도 버튼이 enabled 되는 것을 발견했습니다. 
그리고 직접 실행하는 것보다 테스트 도구를 활용하는 게 속도가 훨씬 빠른 것도 좋았습니다.</p>
<p>물론 직접 다양한 테스트를 해보는 건 필수겠지만 미처 찾지 못하는 오류들을 발견하는 데 도움이 될 것 같습니다. </p>
<blockquote>
<p>참고 자료</p>
</blockquote>
<ul>
<li><a href="https://ui.toast.com/weekly-pick/ko_20210818">https://ui.toast.com/weekly-pick/ko_20210818</a></li>
</ul>