Unity Object Pooling Framework

1. 소개 : 재사용이 빈번한 오브젝트들을 관리하는 과정에서, 반복되는 Instantiate과 Destory를 줄여 성능적 개선을 이루기 위해 사용하는 최적화 방법

2. 설치 방법 : 유니티 프로젝트창에 해당 파일들을 복사

3. 파일 별 설명 :
   

5. 사용 방법 :
   - 오브젝트들이 생성되고 삭제될 씬에 PoolManager 오브젝트를 만들어, PoolManager.cs 컴포넌트를 추가해준다
   - 생성되고 삭제될 오브젝트에, IPoolable 인터페이스를 상속받고, OnSpawn(), OnDespawn()을 구현해준다. 동일 오브젝트에 Poolable.cs 컴포넌트를 추가해준다
   - PoolConfig Scriptable Object를 생성하여 윗줄의 오브젝트 Prefab을 적용하고, initialSize와 expandable 여부를 설정한다
   - 생성된 PoolConfig SO를 맨 윗줄에서 설명한 PoolManager inspector에서 추가해준다.
  
6. 주의 사항 :
   해당 프레임워크는 여러씬에 거쳐 관리되는 DDOL이 아니다. 따라서 각 씬에서 사용될 오브젝트들에 대해 3번의 사용방법을 적용시켜줘야한다.
   이렇게 한 이유는, 여러씬에서 각기 다른 오브젝트들이 사용될텐데, 그렇게 된다면, 게임 내 사용되는 모든 오브젝트들에 대해 해당 내용을 적용시키는건 데이터 낭비라고 생각했기 때문이다. 
