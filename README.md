# 카드 편집기 드래그 인수인계 실험

기존 정적 카드 편집기에 문구 드래그 이동을 추가합니다. 검사는 experiment/protocol.json에 고정한 정확히 10개입니다. 사용 상한은 AI별 30분 / 사용자 작업 메시지 10회입니다. 이 README와 protocol의 동일한 최초 요청을 A와 B 모두에게 제공합니다. A에는 기본 이동 구현 뒤 중단, B에는 같은 목표의 남은 경계 처리 완료라는 역할이 주어집니다.

## 환경과 실행

Node.js 18 이상과 npm, Chromium 또는 Microsoft Edge가 필요합니다. 앱에는 API 키나 환경 비밀값이 없습니다. 브라우저 데이터는 IndexedDB에 로컬로 저장합니다.

저장소 루트에서 `npm ci`를 실행합니다. Edge를 쓰지 않으면 `npx playwright install chromium`으로 검사 브라우저를 설치합니다.

첫 터미널에서 `npm run serve`를 실행하고 http://127.0.0.1:8766 을 엽니다. 두 번째 터미널에서 `npm test`를 실행합니다. Windows Edge 사용 시 먼저 아래 환경 변수를 지정합니다.

```powershell
$env:BROWSER_EXECUTABLE='C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe'
npm test
```

앱 자체는 dist/index.html, dist/app.js, dist/style.css로 구성되며 npm은 검사 도구 설치에 필요합니다. 기본 검사 결과는 qa/results/results.json입니다. 브라우저는 검사 회차마다 새 저장소 컨텍스트를 사용합니다. FAIL이 있으면 종료코드 1, 검사 도구 자체 오류는 종료코드 2입니다.

## 기록

실제 검사 로그와 7항목 인수인계는 저장소 바깥의 인수인계 묶음에 있습니다. A/B 시작·종료 코드는 Git 해시로 기록합니다. B는 다른 모델 또는 서비스의 새 대화에서 저장소와 동일한 인수인계 문서만 받아야 합니다. 이 대화 전문을 제공하지 않습니다.

검사 삭제·완화·기대값 변경은 금지합니다. generated 이미지, 결과 로그, node_modules 및 lockfile은 재작업량 계산에서 제외합니다. 손상 이미지/JSON 처리, 기존 템플릿 기능, Canvas PNG 출력은 보존합니다. 공개 저장소에는 .env, 토큰, 개인 이미지를 넣지 않습니다.

사용자 요청 횟수는 사용자의 작업 메시지 횟수입니다. 도구 호출 횟수나 모델 내부 요청과 합산하지 않습니다. 준비 작업과 A의 작업 시간을 구분하고 실제 시간을 기록합니다. 실패 회차는 고정 10개 검사 결과 중 하나 이상 FAIL인 회차이며, 실패 항목 수와 구분합니다.
