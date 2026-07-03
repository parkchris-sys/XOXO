# Send to All — 개발 진행 현황

## 프로젝트 개요
- **장르**: 회사 그룹웨어 오마주 코미디 승인 게임
- **구조**: 15스테이지 (미니게임 65% + 콤보번들 35%) → 보스전
- **배포**: https://parkchris-sys.github.io/XOXO/
- **파일**: `C:\Users\Chris\Desktop\send-to-all\index.html` (단일 파일)

---

## 완료

### UI/디자인
- [x] 기본 전자결재 그룹웨어 UI 구조
- [x] 상단 HUD (Stage / 결재자 / 타이머 / 진행도)
- [x] 타이머 10초 이하 경고 애니메이션 (빨간 펄스)
- [x] 게임 스킨 리디자인 (네이비 + 앰버 팔레트, 게임 버튼, stamp 연출)
- [x] 실패 화면: "반려" 도장 + shake 애니메이션
- [x] 클리어 화면: "전사 발송 완료" + 승인 도장 + Thanos GIF
- [x] 시작 화면: Send to All 게임 로고, 시작 버튼
- [x] 버튼 hover/pressed 효과 (게임 버튼 스타일)
- [x] SVG 아이콘/스탬프 인라인 적용 (stamp_rejected, stamp_complete 등)
- [x] GitHub Pages 배포 + .nojekyll 추가

### 스테이지 구조
- [x] startRun(): 미니게임 9개 + 콤보번들 5개 = 14스테이지 랜덤 배치
- [x] 연속 같은 타입 방지 셔플 로직
- [x] 단어순서 맞추기 런당 1회 제한 (usedWordOrder 플래그)
- [x] 보스전 `boss_send_to_all` (14스테이지 클리어 후 자동 진입)

### 미니게임 (캔버스/인터랙티브)
| 이름 | 설명 |
|------|------|
| `brick_approval` | 벽돌깨기 |
| `conveyor_approval` | 컨베이어 벨트에서 버튼 수집 |
| `assemble_approval` | 문서 조립 |
| `spot_difference` | 틀린그림찾기 3개 (오답 즉시 실패) |
| `whack_approval` | 두더지 잡기 (초록 정답, 오타 초록 함정) |
| `falling_approval` | 낙하 아이템 받기 |
| `catch_approval` | 빈 박스 기울여서 버튼 떨어뜨리기 |
| `tilt_approval` | 공 굴려 TARGET 구역 홀드 (좌우 FAIL존) |
| `escape_approval` | 장애물 피해 버튼 도달 (충돌 즉시 실패) |
| `excel_approval` | 사내 엑셀 스프레드시트에서 "동의합니다" 셀 찾기 |
| `parking_approval` | WASD 주차 미니게임 (장애물/콘/협소 코스) |
| `DecoyRapidButton` | 페이크 버튼들 사이 숨겨진 진짜 버튼 찾기 |
| `forbidden_zones` | Flappy Bird 형식 (파이프 통과) |
| `running_button` | 도망가는 버튼 잡기 |

### 콤보 번들 (단순 클릭 묶음)
| 이름 | 포함 스텝 풀 |
|------|-------------|
| `combo_button` | greenWrong, typo, swapped, disabled, hover 등 |
| `combo_memory` | memoryFlash, flashing, warningMem, cycling, disappearing |
| `combo_document` | sentence, fix, wordOrderStep, checkboxText 등 |
| `combo_input` | slider, dropdown, checkboxText, holdStep, reverseScrollCheck |
| `combo_fix` | fix(필수) + 랜덤 2개 |
| `combo_review` | typo, sentence, wordOrderStep, fix 등 |
| `combo_tasks` | holdStep, fix, slider, dropdown, reverseScrollCheck 등 |
| `combo_chaos` | 20종 풀에서 4개 랜덤 |
| `combo_scroll` | scrollCheck, reverseScrollCheck, holdStep 등 |

### 개별 스텝 기믹 (콤보 내 사용)
- `greenWrong` / `typo` / `swapped` / `disabled` / `hover` / `cycling` / `flashing`
- `memoryFlash` / `warningMem` / `disappearing` / `sentence` / `fix` / `checkboxText`
- `slider` (정답 범위 랜덤) / `dropdown` / `wordOrderStep` (런당 1회)
- `holdStep` (홀드 중 에러 팝업 함정) / `scrollCheck` (가짜 버튼, 정답 위치 랜덤)
- `reverseScrollCheck` / `popupCancel` / `popupDouble` / `popupGreen`

### 제거된 기믹
- `claw_approval` — 인형뽑기 (너무 쉬움)
- `breakout_approval` — 패들 벽돌깨기 (brick과 중복)
- `hanko_approval` — 이름 고르고 날인 (너무 쉬움)
- `fold_approval` — 접힌 문서 드래그 (반복 요청으로 제거)
- `catch_approval` (구버전) — 낙하 아이템 바구니 (교체됨)

---

## 미결/추가 예정
- [ ] 새 미니게임 아이디어 대기 중 (2슬롯 공석)
- [ ] 모바일 지원 (현재 PC 전용)
- [ ] 깃헙 반영은 명시적 요청 시에만 push

---

## 기술 스펙
- **단일 파일**: HTML/CSS/JS 전부 `index.html`
- **프레임워크 없음**: 바닐라 JS + Canvas API
- **사운드**: `playSound(name, volume)` — Audio.cloneNode() 방식
- **셔플**: Fisher-Yates (반드시 반환값 저장)
- **GitHub**: `parkchris-sys/XOXO`, main 브랜치, Pages 자동 배포
