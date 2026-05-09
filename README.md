# bot-status

자동봇 (TQQQ Fortress × BULZ 50/50 blend, `lhy332/investment-strategy` private repo)
의 헬스 체크 결과를 publish 하는 **public mirror**.

소스 코드: 비공개 (private repo). 이 리포는 **Colab/외부에서 raw URL 로
fetch 가능한 상태 파일** 만 호스트한다.

## Raw URL (Colab 등 anon fetch)

```
https://raw.githubusercontent.com/lhy332/bot-status/main/bot_status.json
```

## Schema

`bot_status.json` 의 구조 (`schema_version=1`):

```json
{
  "schema_version": 1,
  "generated_at_et": "YYYY-MM-DDTHH:MM:SS-0500",
  "generated_at_utc": "YYYY-MM-DDTHH:MM:SSZ",
  "expected_last_close": "YYYY-MM-DD",
  "overall": "ok | warn | err",
  "checks": [
    { "name": "git_sync", "status": "...", "summary": "...", "detail": {...} },
    { "name": "parquet_fresh", ... },
    { "name": "targets_fresh", ... },
    { "name": "launchd_loaded", ... },
    { "name": "last_bot_run", ... }
  ]
}
```

`overall = max(status)` 우선순위: `err > warn > ok`.

## Update cadence

`launchd` 가 평일 09:25 ET 에 봇 실행. 봇 종료 직후 헬스 체크가 자동으로
이 파일을 갱신 + push. 코랩이 주기적으로 fetch 해서 봇 상태 모니터링.

## 노출되는 정보

- 시장 데이터 last_date (yfinance 로 누구나 가능)
- 인프라 메타 (parquet 카운트, launchd label)
- 정책 파라미터 1개 (`current_lev`, 한 숫자)
- short commit SHA (private repo, unguessable)
- 봇 로그 마지막 라인 (홈 디렉토리 경로 sanitized)

PII / 전략 코드 / 계좌 정보는 포함되지 않음.
