#  Unity Object Pooling Framework

## 1. 소개
재사용이 빈번한 오브젝트를 관리할 때 반복적으로 발생하는 `Instantiate()`와 `Destroy()` 호출을 줄여 성능을 개선하기 위한 Unity 오브젝트 풀링 프레임워크입니다.

---

## 2. 설치 방법
프로젝트의 **Assets** 폴더에 해당 스크립트들을 복사하여 사용합니다.

---

## 3. 파일 별 설명

| 파일명 | 역할 |
| :--- | :--- |
| **`IPoolable.cs`** | 풀링되는 오브젝트의 생명주기를 정의하는 인터페이스 (`OnSpawn()`, `OnDespawn()`) |
| **`Poolable.cs`** | 오브젝트가 자신을 Pool로 반환할 수 있도록 연결해주는 컴포넌트 |
| **`ObjectPool.cs`** | 실제 오브젝트를 생성하고, Queue로 보관 및 재사용하는 풀 로직 |
| **`PoolConfig.cs`** | 풀링할 Prefab과 초기 사이즈, 확장 여부를 정의하는 `ScriptableObject` |
| **`PoolManager.cs`** | 여러 `ObjectPool`을 관리하는 중앙 관리자 (Singleton) |

---

## 4. 사용 방법

### **1. 매니저 생성**
오브젝트가 생성/반환될 씬에 빈 오브젝트(Empty Object)를 생성하고 `PoolManager.cs`를 추가합니다.  
<img width="443" height="235" alt="EmptyPoolManager" src="https://github.com/user-attachments/assets/aff7a660-2177-4829-91e1-1c5b325e665a" />

### **2. 오브젝트 설정**
풀링할 오브젝트에 `IPoolable` 인터페이스를 상속받아 `OnSpawn()`, `OnDespawn()`을 정의하고, 동일 오브젝트에 `Poolable.cs` 컴포넌트를 추가합니다.  
<img width="263" height="374" alt="addcomponent" src="https://github.com/user-attachments/assets/b6a18419-6482-4aa4-af8a-36538da656bc" />  <img width="300" height="340" alt="ExScript" src="https://github.com/user-attachments/assets/21a89549-6f1e-4a6c-88e4-7efcb7edfe31" />

### **3. Config 생성 및 설정**
PoolConfig ScriptableObject를 생성하여 Prefab, initialSize, expandable 옵션을 설정합니다.  
<img width="400" height="512" alt="Create" src="https://github.com/user-attachments/assets/073e5319-6129-4603-9177-5c87727ef138" />  <img width="350" height="324" alt="Config" src="https://github.com/user-attachments/assets/20d330f7-2a0e-466f-bf2b-ef9445ae5543" />

### **4. 매니저 등록**
생성한 PoolConfig를 PoolManager의 Inspector에 등록합니다.  
<img width="400" height="600" alt="PoolManager" src="https://github.com/user-attachments/assets/8c628fae-1796-41f6-acb6-5237a3b1ee5c" />


---

## 5. 사용 예시

**오브젝트 생성 (Instantiate 대신)**
```csharp
GameObject bullet = PoolManager.Instance.Get(bulletPrefab);
bullet.transform.position = firePoint.position;
bullet.transform.rotation = firePoint.rotation;
```
---

## 6. 주의 사항 :
    
   이 프레임워크는 DontDestoryOnLoad 기반으로 동작하지 않습니다. 각 씬마다 필요한 오브젝트 풀을 개별적으로 구성해야합니다. 즉, 각씬에 PoolManager를 두고 관리해야합니다.
   이렇게 설계한 이유는 모든 씬에서 사용되지 않는 오브젝트까지 미리 풀링하는 것을 방지하여, 불필요한 메모리 사용을 줄이기 위한 이유입니다.
