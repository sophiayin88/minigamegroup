# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

순수 HTML/CSS/JS(프레임워크·빌드 도구 없음)로 만든 6가지 미니게임 모음 웹앱.
빌드 스텝이 없으며, 각 파일을 브라우저에서 직접 열거나 GitHub Pages로 서빙한다.

- **라이브 URL**: https://sophiayin88.github.io/minigamegroup/
- **GitHub**: https://github.com/sophiayin88/minigamegroup (branch: `main`)

## 개발 방법

빌드·컴파일·패키지 설치 과정이 없다. 파일을 수정한 뒤 브라우저에서 직접 열면 된다.

```powershell
# 로컬에서 파일 열기 (Windows)
start index.html
start snake.html   # 특정 게임 직접 열기
```

변경 후 배포:
```powershell
git add <파일명>
git commit -m "설명"
git push origin main
# push 후 ~1분 내 GitHub Pages 자동 재배포
```

## 파일 구조 및 역할

| 파일 | 역할 |
|------|------|
| `index.html` | 허브 홈페이지. 게임 카드 그리드로 각 게임 진입점 제공 |
| `minesweeper.html` | 지뢰찾기. 초급(9×9)/중급(16×16)/고급(30×16) |
| `snake.html` | 뱀 게임. `<canvas>` 기반, 방향키/WASD, 난이도 3단계 |
| `2048.html` | 2048 퍼즐. 방향키·터치 스와이프 지원 |
| `memory.html` | 메모리 카드. 4×4(쉬움)/6×6(어려움), CSS flip 애니메이션 |
| `tictactoe.html` | 틱택토. 플레이어 vs AI, Minimax + α-β pruning |
| `dresup.html` | 인형 옷 입히기. SVG 레이어 시스템으로 의상 조합 |

## 디자인 시스템 (공통)

모든 파일이 동일한 다크 테마를 공유한다. 새 파일 작성 시 이 값을 사용할 것.

```css
background: #1a1a2e;   /* 페이지 배경 */
background: #16213e;   /* 카드/패널 배경 */
border: #0f3460;       /* 테두리 */
accent: #e94560;       /* 강조색 (빨강) */
text: #e0e0e0;         /* 본문 */
text-muted: #a0a0c0;   /* 보조 텍스트 */
font: 'Segoe UI', sans-serif;
```

공통 UI 패턴:
- **게임 오버레이**: `position:fixed; inset:0` + `.hidden { display:none }` 토글
- **홈 링크**: 각 게임 상단 `<a class="home-link" href="index.html">← 홈으로</a>`
- **난이도 버튼**: `.btn-diff` + `.active` 클래스 토글

## 아키텍처 주의사항

**dresup.html SVG 레이어 순서** — 렌더링 순서가 고정되어 있다. 새 의상 추가 시 이 순서를 지켜야 올바르게 겹쳐 보인다:
1. `#layer-hair-back` (머리카락 뒤쪽)
2. `#body-base` (피부)
3. `#layer-shoes`
4. `#layer-bottom` (하의)
5. `#layer-top` (상의 + 드레스)
6. `#layer-hair-front` (머리카락 앞쪽)
7. `#face` (얼굴 — 항상 최상위)
8. `#layer-acc` (악세서리)

드레스(`dress-*`)와 상의(`top-*`)/하의(`bottom-*`)는 상호 배타적이다 — `selectItem()`에서 상호 해제 로직이 처리된다. SVG 아이템 ID 규칙: `hb-N`(헤어 뒤), `hf-N`(헤어 앞), `top-N`, `bottom-N`, `dress-N`, `shoes-N`, `acc-N`.

**minesweeper.html 첫 클릭 보호** — 지뢰는 첫 번째 클릭 이후에 배치된다 (`firstClick` 플래그). 로직 수정 시 이 순서를 유지해야 한다.

## Git / 배포 규칙

- 파일 변경 후 **반드시 commit + push**한다 (push = 자동 GitHub Pages 재배포).
- 커밋 메시지는 한국어로 간결하게 작성.
- 브랜치는 `main` 단일 브랜치 사용.
