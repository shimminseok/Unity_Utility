# 🎯 Object Pooling System

이 시스템은 Unity 환경에서 반복적으로 생성/삭제되는 GameObject의 성능 문제를 해결하기 위해 제작된 **커스텀 풀링 시스템**입니다.  
`IPoolObject` 인터페이스를 기반으로 유연한 재사용 구조를 지원하며, 다양한 게임 오브젝트에 적용 가능합니다.

---

## 📌 구성 요소

| 파일명              | 설명 |
|--------------------|------|
| `IPoolObject.cs`   | 풀링 대상이 구현해야 하는 공통 인터페이스 정의 |
| `ObjectPoolManager.cs` | 전체 풀의 생성, 반환, 관리 기능을 제공하는 싱글톤 매니저 |

---

## 🛠️ 주요 기능

- 각 오브젝트는 고유한 `PoolID`, `PoolSize` 를 가지며 풀 초기화 시 자동 등록
- `GetObject(string poolId)`로 오브젝트 요청 시 풀에서 꺼내거나 부족하면 추가 생성
- `ReturnObject(GameObject, float delay)`로 일정 시간 후 자동 반환 가능
- 오브젝트 반환 시 `OnReturnToPool()`, 풀에서 꺼낼 시 `OnSpawnFromPool()` 이벤트 자동 호출
- 각 풀은 별도의 부모 오브젝트에 그룹화되어 에디터 내 관리가 쉬움

---

## ✅ 사용 방법

1. `MonoBehaviour` 상속 클래스에 `IPoolObject` 인터페이스 구현
2. `ObjectPoolManager.Instance.GetObject("PoolID")` 로 요청
3. `ObjectPoolManager.Instance.ReturnObject(obj)` 로 반환

```csharp
public class Bullet : MonoBehaviour, IPoolObject
{
    public GameObject GameObject => gameObject;
    public string PoolID => "Bullet";
    public int PoolSize => 20;

    public void OnSpawnFromPool() { /* 초기화 코드 */ }
    public void OnReturnToPool() { /* 정리 코드 */ }
}
