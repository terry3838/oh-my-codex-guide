# 알림 시스템

OMX는 장시간 실행되는 에이전트 작업의 완료, 대기, 질문 요청 등의 이벤트를 외부 채널로 전달하는 알림 시스템을 내장한다. 알림 설정은 `~/.codex/.omx-config.json` 에 저장된다.

---

## 지원 알림 채널

| 채널 | 방식 | 설정 키 |
|------|------|---------|
| Discord | Webhook 또는 Bot | `notifications.discord` |
| Telegram | Bot API | `notifications.telegram` |
| Slack | Webhook | `notifications.slack` |
| OpenClaw | HTTP gateway 또는 CLI command | `notifications.openclaw` |
| Custom Webhook | HTTP POST | `notifications.custom_webhook_command` |
| Custom CLI | 임의 CLI 커맨드 | `notifications.custom_cli_command` |

### 지원 이벤트

| 이벤트 | 설명 |
|--------|------|
| `session-start` | Codex 세션 시작 시 |
| `session-idle` | 에이전트가 대기 상태일 때 |
| `ask-user-question` | 에이전트가 사용자 입력을 요청할 때 |
| `stop` | 세션이 중단될 때 |
| `session-end` | 세션이 완료/종료될 때 |

---

## $configure-notifications 스킬

Codex 세션 내부에서 `$configure-notifications` 스킬을 실행하면 대화형으로 알림 채널을 설정할 수 있다.

```text
$configure-notifications
```

이 스킬은 다음 채널 설정을 안내한다:
- Discord webhook/bot 설정
- Telegram bot token 및 chat ID 설정
- Slack webhook 설정
- OpenClaw gateway 설정

---

## 각 채널 설정 방법

### Discord Webhook

`~/.codex/.omx-config.json` 에 다음 설정을 추가한다:

```json
{
  "notifications": {
    "enabled": true,
    "discord": {
      "enabled": true,
      "webhookUrl": "https://discord.com/api/webhooks/YOUR_WEBHOOK_ID/YOUR_WEBHOOK_TOKEN"
    }
  }
}
```

### Telegram Bot

```json
{
  "notifications": {
    "enabled": true,
    "telegram": {
      "enabled": true,
      "botToken": "YOUR_BOT_TOKEN",
      "chatId": "YOUR_CHAT_ID"
    }
  }
}
```

Telegram Bot Token 발급: [@BotFather](https://t.me/BotFather) 에서 `/newbot` 커맨드로 발급.
Chat ID 확인: `https://api.telegram.org/bot{TOKEN}/getUpdates` API 호출 후 `chat.id` 값 사용.

### Slack Webhook

```json
{
  "notifications": {
    "enabled": true,
    "slack": {
      "enabled": true,
      "webhookUrl": "https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK"
    }
  }
}
```

Slack Incoming Webhook URL 발급: [Slack API](https://api.slack.com/apps) 에서 앱 생성 후 Incoming Webhooks 활성화.

---

## OpenClaw 통합

OpenClaw는 OMX의 가장 강력한 알림 통합으로, HTTP 게이트웨이 또는 CLI command를 통해 에이전트 이벤트를 처리한다.

### 활성화 환경변수

```bash
# OpenClaw 파이프라인 필수 활성화
export OMX_OPENCLAW=1

# command gateway 추가 활성화
export OMX_OPENCLAW_COMMAND=1

# command gateway 타임아웃 설정 (ms, 기본값 5000)
export OMX_OPENCLAW_COMMAND_TIMEOUT_MS=120000

# OpenClaw 인증 토큰
export HOOKS_TOKEN="your-openclaw-hooks-token"
```

### Option A: HTTP Gateway (openclaw 스키마)

```json
{
  "notifications": {
    "enabled": true,
    "openclaw": {
      "enabled": true,
      "gateways": {
        "local": {
          "type": "http",
          "url": "http://[REDACTED_PHONE]:18789/hooks/agent",
          "headers": {
            "Authorization": "Bearer ${HOOKS_TOKEN}"
          }
        }
      },
      "hooks": {
        "session-end": {
          "enabled": true,
          "gateway": "local",
          "instruction": "OMX task completed for {{projectPath}}"
        },
        "ask-user-question": {
          "enabled": true,
          "gateway": "local",
          "instruction": "OMX needs input: {{question}}"
        }
      }
    }
  }
}
```

### Option B: Generic Alias (custom_webhook_command)

OpenClaw 또는 커스텀 서비스에 사용할 수 있는 범용 별칭:

```json
{
  "notifications": {
    "enabled": true,
    "custom_webhook_command": {
      "enabled": true,
      "url": "http://[REDACTED_PHONE]:18789/hooks/agent",
      "method": "POST",
      "headers": {
        "Authorization": "Bearer ${HOOKS_TOKEN}"
      },
      "events": ["session-end", "ask-user-question"],
      "instruction": "OMX event {{event}} for {{projectPath}}"
    }
  }
}
```

### Option C: CLI Command Gateway (clawdbot 연동, 권장)

에이전트 턴을 직접 트리거하는 방식:

```json
{
  "notifications": {
    "enabled": true,
    "verbosity": "verbose",
    "openclaw": {
      "enabled": true,
      "gateways": {
        "local": {
          "type": "command",
          "command": "(clawdbot agent --session-id omx-hooks --message {{instruction}} --thinking minimal --deliver --reply-channel discord --reply-to 'channel:YOUR_CHANNEL_ID' --timeout 120 --json >>/tmp/omx-openclaw-agent.jsonl 2>&1 || true)",
          "timeout": 120000
        }
      },
      "hooks": {
        "session-start": {
          "enabled": true,
          "gateway": "local",
          "instruction": "[session-start|exec]\nproject={{projectName}} session={{sessionId}} tmux={{tmuxSession}}\n요약: 시작 맥락 1문장\n우선순위: 지금 할 일 1~2개"
        },
        "session-end": {
          "enabled": true,
          "gateway": "local",
          "instruction": "[session-end|exec]\nproject={{projectName}} session={{sessionId}} tmux={{tmuxSession}} reason={{reason}}\n성과: 완료 결과 1~2문장\n검증: 확인/테스트 결과\n다음: 후속 액션 1~2개"
        }
      }
    }
  }
}
```

> 💡 중요: command 끝에 `|| true` 를 추가하여 clawdbot 실패가 OMX 세션을 중단하지 않도록 한다.

### 우선순위 규칙

`notifications.openclaw` 와 `custom_webhook_command` 가 동시에 설정된 경우:
1. `notifications.openclaw` 가 우선 적용됨
2. `custom_webhook_command` / `custom_cli_command` 는 무시됨
3. OMX가 경고 메시지를 출력함

### Template 변수

훅 instruction에서 사용 가능한 변수:

| 변수 | 설명 |
|------|------|
| `{{sessionId}}` | 세션 식별자 |
| `{{tmuxSession}}` | tmux 세션 이름 |
| `{{projectName}}` | 프로젝트 이름 |
| `{{projectPath}}` | 프로젝트 경로 |
| `{{question}}` | 사용자 질문 내용 (`ask-user-question` 전용) |
| `{{reason}}` | 종료 사유 (`session-end` 전용) |
| `{{event}}` | 이벤트 이름 |

---

## CCNotifier 상세도 수준

`verbosity` 설정으로 알림 내용의 상세도를 조절한다:

```json
{
  "notifications": {
    "verbosity": "session"
  }
}
```

| 레벨 | 설명 | 권장 상황 |
|------|------|----------|
| `minimal` | 매우 짧은 신호만 전달 (고신호, 저서술) | 알림이 많을 때 |
| `session` | 간결한 운영 맥락 포함 (권장 기본값) | 일반 사용 |
| `verbose` | 상태 + 액션 + 리스크 프레이밍 포함 | 중요한 세션 모니터링 |

### verbose 프로파일 예시

```json
{
  "notifications": {
    "verbosity": "verbose",
    "openclaw": {
      "hooks": {
        "session-idle": {
          "instruction": "[session-idle|exec]\nsession={{sessionId}} tmux={{tmuxSession}}\n요약: idle 원인 1문장\n복구계획: 즉시 조치 1~2개\n의사결정: 사용자 입력 필요 여부"
        },
        "ask-user-question": {
          "instruction": "[ask-user-question|exec]\nsession={{sessionId}} tmux={{tmuxSession}} question={{question}}\n핵심질문: 필요한 답변 1문장\n영향: 미응답 시 영향 1문장\n권장응답: 가장 빠른 답변 형태"
        }
      }
    }
  }
}
```

---

## 실전 알림 설정 팁

### 1. 토큰은 환경변수로 관리

```bash
# ~/.bashrc 또는 ~/.zshrc 에 추가
export HOOKS_TOKEN="your-token-here"
export OMX_OPENCLAW=1
export OMX_OPENCLAW_COMMAND=1
```

설정 파일에 토큰을 직접 하드코딩하지 말고 환경변수로 참조한다.

### 2. Discord는 channel ID 사용 권장

```bash
# 채널 별칭(#channel-name) 대신 ID 사용
--reply-to 'channel:[REDACTED_PHONE]'
```

채널 별칭은 bot 캐시 상태에 따라 실패할 수 있다.

### 3. OpenClaw 연결 검증

```bash
# Wake 스모크 테스트
curl -sS -X POST http://[REDACTED_PHONE]:18789/hooks/wake \
  -H "Authorization: Bearer ${HOOKS_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"text":"OMX wake smoke test","mode":"now"}'

# 배포 검증
curl -sS -X POST http://[REDACTED_PHONE]:18789/hooks/agent \
  -H "Authorization: Bearer ${HOOKS_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"message":"OMX delivery verification","event":"session-end","sessionId":"manual-check"}'
```

### 4. 로그로 디버깅

```bash
# JSONL 로그 확인
tail -n 50 /tmp/omx-openclaw-agent.jsonl | jq -s '.[] | {timestamp, status: (.status // .error // "ok")}'

# 오류 검색
grep '"error"\|"failed"\|"timeout"' /tmp/omx-openclaw-agent.jsonl | tail -20
```

### 5. Preflight 체크리스트

```bash
# 토큰 확인
test -n "$HOOKS_TOKEN" && echo "token ok" || echo "token missing"

# 환경변수 확인
test "$OMX_OPENCLAW" = "1" && echo "OMX_OPENCLAW=1 ok" || echo "missing OMX_OPENCLAW=1"
test "$OMX_OPENCLAW_COMMAND" = "1" && echo "OMX_OPENCLAW_COMMAND=1 ok" || echo "missing"

# 게이트웨이 연결 확인
curl -sS -o /dev/null -w "HTTP %{http_code}\n" http://[REDACTED_PHONE]:18789 || echo "gateway unreachable"
```

### 6. 오류 진단 가이드

| 오류 | 원인 | 해결 |
|------|------|------|
| HTTP 401/403 | 토큰 누락 또는 잘못됨 | `HOOKS_TOKEN` 환경변수 확인 |
| HTTP 404 | 경로 오류 | `/hooks/agent` 경로 확인 |
| HTTP 5xx | 게이트웨이 런타임 오류 | 게이트웨이 로그 확인 |
| Connection refused | 호스트/포트/방화벽 문제 | OpenClaw 서버 실행 상태 확인 |
| Command disabled | 환경변수 미설정 | `OMX_OPENCLAW=1`, `OMX_OPENCLAW_COMMAND=1` 설정 |
| SIGTERM 종료 | 타임아웃 초과 | `timeout` 값 120000으로 증가 |
| 로그 없음 | 출력 리디렉션 누락 | `>> /tmp/omx-openclaw-agent.jsonl` 추가 |
