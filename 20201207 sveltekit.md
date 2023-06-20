---
date: 2020.12.07
title: sveltekit
tag: sveltekit
---


# sveltekit

### 초고속 프레임워크의 등장

 sapper의 개발자가 sapper 프로젝트를 중단하고, snowpack 기반의 sveltekit 이라는 후속 프레임워크로 전환하는 것 같다. sveltekit은 아직 개발 중이긴 하지만 어느 정도 체험해볼 순 있었다.

주요 기능은 sapper와 유사하지만 훨씬 빠른 것 같다.

- 파일 기반의 라우팅
- 파일명으로부터 파라미터 받기
- 레이아웃, 에러 페이지
- .js 파일로 백엔드 API 만들기
- SSR
- 서비스워커

### Tailwindcss 적용

기존에 sapper나 svelte에 tailwind를 적용했던 방식으로는 제대로 동작하지 않았다. 빌드도 안되고 @apply도 캐쉬를 매번 지워줘야 적용되었다. CDN을 이용한 방법은 잘 되지만 @apply 를 쓸 수가 없어서... 어쩌면 좋아.

찾아보니 누군가가 sveltekit에 적용가능한 방법을 [깃헙](https://github.com/babichjacob/svelte-add-tailwindcss)에 올려두었다. 이따가 해봐야겠다.

### 참고

- [What's the deal with SvelteKit?](https://svelte.dev/blog/whats-the-deal-with-sveltekit)
- [Sapper is dead! What's next in Svelte?](https://www.codingwithjesse.com/blog/sapper-is-dead-whats-next-in-svelte/)
- [sapper faq](https://sapper.svelte.dev/faq)
- [Add Tailwind CSS to Svelte](https://github.com/babichjacob/svelte-add-tailwindcss)