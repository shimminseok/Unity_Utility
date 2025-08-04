# 🧠 Stat Management System

이 시스템은 RPG나 전투 게임에서 유닛의 능력치를 효율적으로 관리하고, **회복/소모/버프/장비 효과**를 통합 처리하기 위한 구조로 설계되었습니다.

---

## 📌 구성 요소

| 파일명             | 설명 |
|-------------------|------|
| `StatBase.cs`     | 모든 스탯의 기본 베이스 클래스 정의 |
| `CalculatedStat.cs` | 공격력, 방어력 등 연산 가능한 스탯 클래스 |
| `ResourceStat.cs` | HP, MP, 쉴드 등 리소스 기반 스탯 클래스 |
| `StatManager.cs`  | 유닛의 모든 스탯을 관리하는 중심 클래스 |
| `StatData.cs`     | 테이블이나 JSON으로부터 불러올 수 있는 스탯 데이터 구조 |
| `IStatProvider.cs`| 초기 스탯 데이터를 제공하는 인터페이스 |

---

## 🛠️ 주요 기능

- `StatManager`는 `IStatProvider`로부터 스탯 초기값을 받아 전투 시작 시 세팅
- 스탯은 두 종류로 분류됨:
  - `CalculatedStat`: 공격력, 속도 등 계산 가능한 수치
  - `ResourceStat`: HP, MP 등 현재/최대값을 가지는 리소스형 수치
- `StatManager`는 다음과 같은 기능 제공:
  - `Recover` / `Consume` : 자원 회복 및 소모
  - `ApplyStatEffect` : 장비, 버프, 레벨업 등 외부 영향 적용
  - `GetValue`, `GetStat<T>` : 실시간 스탯 접근

---

## ✅ 사용 예시

```csharp
// 현재 HP 가져오기
float hp = statManager.GetValue(StatType.CurHp);

// MP 10 소모
statManager.Consume(StatType.CurMp, StatModifierType.Base, 10f);

// 공격력 +5 증가 (장비 효과)
statManager.ApplyStatEffect(StatType.Attack, StatModifierType.Equipment, 5f);
```
## 💡 설계 포인트
- 스탯 타입과 수치 적용 방식을 enum으로 분리하여 확장성과 가독성 강화
- CalculatedStat은 Base + Buff + Equip 구조로 세분화하여 효과 분리 적용 가능
- ResourceStat은 퍼센트 회복/소모를 기본 지원하여 다양한 스킬 연동 가능
- OnValueChanged 이벤트를 통해 UI나 이펙트 시스템과 연동 용이

## 🧩 통합 사용 예시
이 스탯 시스템은 다음과 같은 시스템과 결합해 사용됩니다:
- 전투 FSM → 스탯 변화 및 UI 갱신
- 장비 시스템 → 착용 시 스탯 변동 반영

