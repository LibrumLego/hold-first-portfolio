# 공개 가능한 코드 발췌

아래 예시는 실제 구현에서 사용한 패턴을 짧게 발췌한 것입니다. 전체 앱 소스가 아니며, 앱 식별자·콘솔 값·운영 설정은 포함하지 않았습니다.

## 플랫폼 저장소 우선, 브라우저 저장소 대체

```ts
import { Storage } from '@apps-in-toss/web-framework'

async function read(key: string) {
  try {
    return await Storage.getItem(key)
  } catch {
    return localStorage.getItem(key)
  }
}

async function write(key: string, value: string) {
  try {
    await Storage.setItem(key, value)
  } catch {
    localStorage.setItem(key, value)
  }
}
```

서비스 고유 저장 키는 생략했습니다.

## 숙려 만료 상태 계산

```ts
type HoldStatus = 'HOLDING' | 'DECISION_DUE' | 'SKIPPED' | 'PURCHASED' | 'DELETED'

type HoldItem = {
  status: HoldStatus
  decisionDueAt: string
}

function currentStatus(item: HoldItem, now = Date.now()): HoldStatus {
  const dueAt = new Date(item.decisionDueAt).getTime()
  return item.status === 'HOLDING' && dueAt <= now
    ? 'DECISION_DUE'
    : item.status
}
```

이 발췌본은 검토용이며 운영 저장소·앱 설정·전체 화면 코드를 포함하지 않습니다.
