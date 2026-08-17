# 배포 가이드

CatHome은 빌드 과정이 없는 정적 사이트입니다. **폴더째 올리면 끝**입니다.

## 올려야 하는 파일

| 파일 | 필수 | 설명 |
|---|---|---|
| `index.html` | ✅ | 게임 본체 (HTML·CSS·JS 전부) |
| `CAT.webp` | ✅ | 고양이 사진 스프라이트. **없으면 벡터 드로잉으로 대체**되지만 그림이 달라집니다 |
| `og.png` | ⬜ | 링크 공유 미리보기 이미지 (1200×630) |
| `apple-touch-icon.png` | ⬜ | iOS 홈 화면 아이콘 |
| `README.md`, `DEPLOY.md` | ⬜ | 문서. 올려도 무해하지만 빼도 됩니다 |

외부 CDN·라이브러리·폰트를 전혀 쓰지 않으므로 이 파일들만 있으면 어디서든 동작합니다.

---

## 방법 1. Cloudflare Pages (추천)

무료, 무제한 대역폭, 전 세계 CDN. `이름.pages.dev` 주소를 줍니다.

1. https://dash.cloudflare.com 접속 → 계정 생성/로그인
2. 왼쪽 메뉴 **Workers & Pages** → **Create** → **Pages** 탭 → **Upload assets**
3. 프로젝트 이름 입력 (예: `cathome`) → **Create project**
4. `CatHome` 폴더를 **통째로 드래그&드롭** → **Deploy site**
5. 몇 초 뒤 `https://cathome.pages.dev` 가 나옵니다

**업데이트할 때:** 같은 프로젝트에서 **Create new deployment** → 폴더 다시 드래그.

## 방법 2. Netlify Drop (가장 빠름)

1. https://app.netlify.com/drop 접속
2. `CatHome` 폴더를 브라우저 창에 드래그
3. 즉시 `https://랜덤이름.netlify.app` 주소가 나옵니다

로그인 없이도 되지만, **계정 없이 올리면 주소를 나중에 관리할 수 없습니다.** 계속 쓸 거라면 로그인 후 배포하고 Site settings에서 이름을 바꾸세요.

---

## 배포 직후 할 일

`index.html`의 `<head>`에 있는 공유용 메타태그는 지금 **상대 경로**(`og.png`)로 되어 있습니다.
디스코드·슬랙은 이대로도 미리보기가 뜨지만, **카카오톡·트위터는 절대 URL을 요구**합니다.

도메인이 정해지면 아래 두 줄을 실제 주소로 바꿔주세요.

```html
<meta property="og:image" content="https://내주소.pages.dev/og.png">
<meta name="twitter:image" content="https://내주소.pages.dev/og.png">
```

같은 위치에 이 줄도 추가하면 좋습니다.

```html
<meta property="og:url" content="https://내주소.pages.dev/">
```

카카오톡은 미리보기를 캐시하므로, 수정 후에도 옛 이미지가 보이면
[카카오 개발자 도구](https://developers.kakao.com/tool/clear/og)에서 캐시를 초기화하세요.

---

## 알아둘 점

- **저장은 기기별로 따로입니다.** 진행 상황은 브라우저 `localStorage`(키 `cathome.save.v1`)에 저장되므로, 접속한 사람마다 자기 고양이를 따로 키웁니다. 서버·계정·DB가 필요 없습니다.
- **서버 코드가 없어 무료 요금제로 충분합니다.** 방문자가 늘어도 정적 파일 전송만 발생합니다.
- **HTTPS가 필요합니다.** 위 두 서비스 모두 자동으로 붙습니다. (WebAudio 소리 재생이 http에서는 막힐 수 있습니다)
- **업데이트가 바로 안 보일 때:** 브라우저 캐시 때문입니다. 강력 새로고침(Ctrl+F5)을 안내하거나, 배포 시 파일명을 바꾸는 방식으로 우회할 수 있습니다.
