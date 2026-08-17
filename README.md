# nyangdonjang-guide

어나더에덴(Another Eden) 냥도장(Cat Stamp) 위치 가이드. 74개 냥도장을 시대/대륙별로 검색하고 이미지로 확인할 수 있는 SPA.

## Tech Stack

- 순수 HTML + inline CSS + vanilla JS (빌드 도구 없음)
- Noto Sans KR 웹폰트
- GitHub Pages 정적 배포

## 구조

```
nyangdonjang-guide/
├── index.html      # SPA 전체 (925줄)
├── images/         # WebP 스크린샷 163장 (이 중 145장이 데이터에서 참조됨)
└── _dist/          # 파생 산출물, gitignore (아래 "파생본" 참조)
```

`images/`에 참조되지 않는 18장이 남아 있습니다. 같은 위치를 다른 제보자가 중복 제보한 것으로,
등록 판정에서 SKIP 된 이력입니다 (파일은 판단 근거로 보존).

## 동작 방식

### 데이터
- `const D=[...]` JS 배열로 index.html 내부에 인라인
- 스키마: `{e: 시대, c: 대륙, l: 장소명, d: 설명, i: [이미지파일명], a: 기여자}`
- 이미지는 `images/` 폴더에서 상대경로 참조 (모두 WebP 포맷)

### 주요 기능
- **필터**: 시대(고대/현대/미래) / 대륙(미그레이나/가를레아/기타) — 각각 "전체" 옵션 포함
- **검색**: 장소명, 설명 텍스트 검색
- **뷰 전환**: 카드 뷰 / 리스트 뷰
- **모달 + 라이트박스**: 이미지 상세 보기
- **해시 라우팅**: 브라우저 뒤로가기 지원

### 디자인
- Notion 스타일 밝은 테마 (`background: #f8f7f4`)
- grain texture overlay
- max-width: 1080px, 반응형 레이아웃

## 배포

배포처가 두 곳입니다. **둘 다 갱신해야 합니다.**

| 배포처 | 주소 | 갱신 방법 |
|---|---|---|
| GitHub Pages | https://ggobugithesquirtle.github.io/nyangdonjang-guide/ | `git push` 하면 자동 |
| Netlify (미러) | https://nyangdonjang-guide.netlify.app/ | `netlify deploy --prod --dir=.` **수동** |

Netlify는 git 연동이 안 걸려 있어 push 만으로는 갱신되지 않습니다.
한국에서의 응답 속도는 Pages 쪽이 5배 빠르므로(TTFB 49ms vs 251ms), 이미지 주소는 Pages 를 씁니다.

진입점은 `index.html`. Pages 는 CORS 가 열려 있어(`Access-Control-Allow-Origin: *`)
외부 사이트에서 이미지를 절대 URL 로 불러다 쓸 수 있습니다.

## 파생본 (`_dist/`)

`index.html` 이 단일 원본이고, 파생본은 아래로 생성합니다. `_dist/` 는 gitignore 대상입니다.

```bash
py -3.12 _tools/build_nyangdonjang_dist.py
```

| 산출물 | 용도 |
|---|---|
| `portable/nyangdonjang-absolute.html` | 이미지를 Pages 절대 URL 로 치환 (58KB). 외부 사이트·Gemini 캔바스용 |
| `portable/nyangdonjang-inline.html` | 이미지 base64 내장 (8.3MB). **Claude Artifact 는 외부 호스트를 CSP 로 차단**하므로 이쪽 필수 |
| `cafe-handover/` + `냥도장_자료_인계본.zip` | 카페 운영자 인계용. jpg 변환 + 위치명 파일명 + 목록표(md/csv) + 이미지 주소 안내 |

## 개발 노트

- CSS/JS 모두 HTML 내 인라인 (단일 파일 배포 원칙)
- 이미지 추가 시: `images/`에 WebP 파일 추가 + `const D` 배열에 데이터 항목 추가
- 이미지 원본은 Discord에서 수집 후 `_tools/discord_to_nyangdonjang.py`로 변환
  (증분 fetch. 상태는 `_tools/nyangdonjang_fetch_state.json`)
- 중복 제보 판정은 **장소명이 아니라 냥도장 대사**로 — 같은 장소는 게임 내 대사가 동일합니다
- Catbox.moe 외부 호스팅에서 로컬 WebP로 마이그레이션 완료 (외부 이미지 호스팅 금지 정책)
- 데이터 항목 수를 바꿨으면 `<meta property="og:description">` 의 개수 표기도 같이 고칠 것
