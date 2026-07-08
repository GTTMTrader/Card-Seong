# 카드 리포트 (개인용)

카드 소비 실적 분석 + 카드별 최적 결제 판정 + 전월실적 트래킹 + 스샷 OCR 일괄입력 웹앱.
순수 HTML/CSS/JS 단일 파일(`index.html`)이라 서버나 빌드 과정 없이 정적 호스팅만 하면 바로 동작함.

## GitHub Pages로 배포하기

1. 이 폴더 내용을 새 저장소에 push (파일명 `index.html` 그대로 유지 - 루트에 있어야 함)
   ```bash
   git init
   git add .
   git commit -m "init"
   git branch -M main
   git remote add origin <레포 URL>
   git push -u origin main
   ```
2. GitHub 저장소 → Settings → Pages → Source를 "Deploy from a branch" → Branch: `main` / `/(root)` 선택 → Save
3. 몇 분 뒤 `https://<username>.github.io/<repo명>/` 로 접속 가능

## iPhone에 앱처럼 설치하기

1. 위 URL을 Safari로 열기
2. 공유 버튼 → "홈 화면에 추가"
3. 홈 화면 아이콘으로 실행 (일반 앱처럼 전체화면으로 뜸)

## 데이터 저장 방식

- 모든 결제 데이터는 브라우저 `localStorage`에 저장됨 (기기 로컬, 서버로 전송 안 됨)
- GitHub Pages 도메인으로 접속해야 데이터가 안정적으로 유지됨 (파일을 직접 열면 브라우저 정책상 저장이 불안정할 수 있음 - 그래서 이 방식으로 배포하는 게 맞음)
- 기기를 바꾸거나 데이터를 백업하고 싶으면 앱 내 "설정" 탭 → Export/Import 기능으로 JSON 텍스트를 옮기면 됨

## 스샷 자동 인식(OCR) 기능

- "일괄" 탭에서 카드사 앱 이용내역 스샷을 올리면 [Tesseract.js](https://github.com/naptha/tesseract.js)(jsDelivr CDN)로 텍스트를 자동 인식함
- 처음 실행 시 한글 인식 데이터(수 MB)를 인터넷에서 내려받기 때문에 첫 실행은 다소 걸림. 이후엔 브라우저 캐시로 빨라짐
- 인식이 100% 정확하지 않을 수 있어서, 저장 전에 항상 미리보기 표에서 확인/수정하도록 만들어둠

## 카드 혜택 데이터(`card_db.json`) 업데이트하기

`index.html` 안의 `CARD_DB` 객체가 실제 계산 로직에 쓰이는 원본임 (`card_db.json`은 참고/백업용 문서).
카드 혜택이 바뀌거나(연회비 인상, 프로모션 등) 실적 조건이 바뀌면 `index.html`의 `CARD_DB` 부분을 직접 수정하면 됨.

## 현재 등록된 카드 (5)

| 카드 | 연회비 | 전월실적 |
|---|---|---|
| 신한 미스터라이프 | 15,000원 | 30만원 |
| BC 마카오 | 5,000원 (미확인 근사치) | 15만원 |
| 롯데 디지로카 라스베가스 | 20,000원 | 무실적 |
| BC 나라사랑IBK (체크카드) | 0원 | 8만원 |
| 현대 스마일카드 | 20,000원 | 30만원 |

## 알려진 한계

- 할인율 계산은 건당/월 한도, 시간대(나이트/주말) 조건을 단순화한 근사치임
- BC 마카오 연회비는 미확인 상태 - 실제 카드 명세서로 확인 후 `index.html`에 반영 필요
- BC 나라사랑IBK는 군 복무자 전용 체크카드라 "최적 카드 추천" 로직에서 제외됨
