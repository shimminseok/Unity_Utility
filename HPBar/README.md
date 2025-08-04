# ❤️ HP Bar System

이 시스템은 전투 중 유닛의 체력, 실드를 실시간으로 보여주는 **모듈형 체력바 UI 시스템**입니다.  
풀링과 상태 이벤트 기반으로 구현되어 퍼포먼스를 고려한 구조입니다.

---

## 📌 구성 요소

| 파일명               | 설명 |
|----------------------|------|
| `HPBarUI.cs`         | 실제 HP Bar UI의 표시 및 이벤트 반응 처리 |
| `HealthBarManager.cs`| HP Bar의 생성/삭제 및 위치 업데이트 관리 |

---

## 🧠 주요 기능

- **체력 + 실드 분리 렌더링**: 현재 체력과 실드를 분리하여 UI에 반영
- **풀링 시스템 연동**: `ObjectPoolManager`를 통해 HP Bar 재사용
- **카메라 기준 위치 보정**: `LateUpdate`에서 스크린 위치 갱신
- **Shader 기반 분리선 효과**: 체력 변화 시 UI 몰입도 향상

---

## 🧩 연동 시스템

- `StatManager`의 `OnValueChanged` 이벤트와 연동되어 UI 자동 갱신

---

## ✅ 사용 예시

```csharp
// 유닛 생성 시 체력바 생성
HPBarUI hpBar = HealthBarManager.Instance.SpawnHealthBar(unit);

// 전투 종료 시 회수
hpBar.UnLink();  // 내부적으로 풀링 처리
```

## 💡 최적화 포인트
- 풀링 시스템 도입: 매 전투마다 UI 생성/파괴를 피하고 메모리/성능 최적화
- LateUpdate 위치 업데이트: 카메라와의 싱크를 위해 최적의 타이밍에 위치 반영
- OnValueChanged 기반 갱신: 불필요한 매 프레임 갱신 제거
- 감정 시스템 연동: UI에서 감정 상태를 실시간 피드백으로 제공
