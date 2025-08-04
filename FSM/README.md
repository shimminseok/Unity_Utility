# 🧩 Finite State Machine (FSM)

이 시스템은 유닛의 상태(Idle, Attack, Hit, Die 등)를 명확하게 분리하고 전환하기 위해 제작된 **범용 FSM 구조**입니다.  
Unity의 `MonoBehaviour`를 기반으로, 어떤 컴포넌트든 손쉽게 상태 머신을 구현할 수 있도록 제네릭 기반으로 설계되었습니다.

---

## 📌 구성 요소

| 파일명            | 설명 |
|------------------|------|
| `StateMachine.cs` | 상태 전환 및 상태별 메서드 실행을 담당하는 FSM 핵심 클래스 |
| `IState.cs`       | 각 상태가 구현해야 하는 인터페이스 (Enter, Update, Exit 포함) |

---

## 🛠️ 주요 기능

- `StateMachine<T, TState>`는 `MonoBehaviour`를 상속한 오브젝트에서 사용 가능
- 각 상태는 `IState<T, TState>`를 구현하여 `OnEnter`, `OnUpdate`, `OnFixedUpdate`, `OnExit` 메서드 제공
- `ChangeState()`를 통해 상태 전환 시 자동으로 이전 상태 종료 → 새 상태 진입 흐름 처리
- `Update()`와 `FixedUpdate()`는 MonoBehaviour에서 위임받아 각 상태별 로직 분리 실행 가능

---

## ✅ 사용 예시

```csharp
public enum EnemyState { Idle, Chase, Attack, Dead }

public class IdleState : IState<EnemyController, EnemyState>
{
    public void OnEnter(EnemyController owner) { /* 초기화 */ }
    public void OnUpdate(EnemyController owner) { /* 대기 로직 */ }
    public void OnFixedUpdate(EnemyController owner) { }
    public void OnExit(EnemyController owner) { /* 정리 */ }
}

// Controller 내부
private StateMachine<EnemyController, EnemyState> stateMachine;

void Start()
{
    stateMachine = new StateMachine<EnemyController, EnemyState>();
    stateMachine.Setup(this, new IdleState());
}

void Update() => stateMachine.Update();
void FixedUpdate() => stateMachine.FixedUpdate();
```
## 💡 설계 포인트
- 상태 별 완전한 역할 분리: 행동, 애니메이션, 이펙트 등을 State 단위로 독립 처리-
- 유닛 FSM, 턴 FSM 등 중복 사용 가능: T와 TState 제네릭으로 다양한 유닛 타입/상태 enum 대응
- OnFixedUpdate 지원: 물리 기반 상태 처리(이동 등)도 고려
