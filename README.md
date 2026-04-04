# nyangdonjang-guide

어나더에덴(Another Eden) 냥도장(Cat Stamp) 위치 가이드. 67개 냥도장을 시대/대륙별로 검색하고 이미지로 확인할 수 있는 SPA.

## Tech Stack

- 순수 HTML + inline CSS + vanilla JS (빌드 도구 없음)
- Noto Sans KR 웹폰트
- GitHub Pages 정적 배포

## 구조

```
nyangdonjang-guide/
├── index.html      # SPA 전체 (890줄)
└── images/         # 100개 WebP 이미지 (냥도장 위치 스크린샷)
```

## 동작 방식

### 데이터
- `const D=[...]` JS 배열로 index.html 내부에 인라인
- 스키마: `{e: 시대, c: 대륙, l: 장소명, d: 설명, i: [이미지파일명], a: 기여자}`
- 이미지는 `images/` 폴더에서 상대경로 참조 (모두 WebP 포맷)

### 주요 기능
- **필터**: 시대(현대/고대/미래/이공간 등) / 대륙별 필터
- **검색**: 장소명, 설명 텍스트 검색
- **뷰 전환**: 카드 뷰 / 리스트 뷰
- **모달 + 라이트박스**: 이미지 상세 보기
- **해시 라우팅**: 브라우저 뒤로가기 지원

### 디자인
- Notion 스타일 밝은 테마 (`background: #f8f7f4`)
- grain texture overlay
- max-width: 1080px, 반응형 레이아웃

## 배포

- **GitHub Pages**: 독립 repo로 배포 중
- 진입점: `index.html`

## 개발 노트

- CSS/JS 모두 HTML 내 인라인 (단일 파일 배포 원칙)
- 이미지 추가 시: `images/`에 WebP 파일 추가 + `const D` 배열에 데이터 항목 추가
- 이미지 원본은 Discord에서 수집 후 `_tools/discord_to_nyangdonjang.py`로 변환
- Catbox.moe 외부 호스팅에서 로컬 WebP로 마이그레이션 완료
