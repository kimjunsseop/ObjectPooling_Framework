Unity Object Pooling Framework

1. 소개 :
   
   재사용이 빈번한 오브젝트를 관리할때 반복적으로 발생하는 Instantiate()과 Destroy() 호출을 줄여 성능을 개선하기 위한 Unity 오브젝트 풀링 프레임워크.


   
2. 설치 방법 :
   
   프로젝트의 Assets 폴더에 해당 스크립트들을 복사하여 사용.



3. 파일 별 설명 :
   
   IPoolable.cs : 풀링되는 오브젝트의 생명주기를 정의하는 인터페이스. (OnSpawn(), OnDespawn())
   Poolable.cs : 오브젝트가 자신을 Pool로 반환할 수 있도록 연결해주는 컴포넌트
   ObjectPool.cs : 실제 오브젝트를 생성하고, Queue로 보관, 재사용하는 풀 로직
   PoolConfig.cs : 풀링할 Prefab과 초기 사이즈, 확장 여부를 정의하는 ScritpableObject
   PoolManager.cs : 여러 ObjcetPool을 관리하는 중앙 관리자 (Singleton)
   


4. 사용 방법 :
   
   - 오브젝트가 생성/반환될 씬에 PoolManager(EmptyObject) 오브젝트를 생성하고 PoolManager.cs를 추가.
   - 풀링할 오브젝트에 IPoolable 인터페이스를 상속받고, OnSpawn(), OnDespawn()을 정의하고(재생성 및 반환될때 처리해줘야할것들), 동일 오브젝트에 Poolable.cs 컴포넌트를 추가.
   - PoolConfig ScriptableObject를 생성하여, 위 줄의 오브젝트 Prefab, initialSize, expandable 옵션을 설정.
   - 생성한 PoolConfig를 PoolManager의 Inspector에 등록.


  
5. 사용 예시 :
    
   오브젝트 생성 (Instantiate 대신) 예시 코드 :
   GameObject bullet = PoolManager.Instance.Get(bulletPrefab);
   bullet.transform.position = firePoint.position;
   bullet.transform.rotation = firePoint.rotation;

   오브젝트 반환 (Destroy 대신) 예시 코드 :
   PoolManager.Instance.Return(bullet);



6. 주의 사항 :
    
   이 프레임워크는 DontDestoryOnLoad 기반으로 동작하지 않습니다. 각 씬마다 필요한 오브젝트 풀을 개별적으로 구성해야합니다. 즉, 각씬에 PoolManager를 두고 관리해야합니다.
   이렇게 설계한 이유는 모든 씬에서 사용되지 않는 오브젝트까지 미리 풀링하는 것을 방지하여, 불필요한 메모리 사용을 줄이기 위한 이유입니다.
