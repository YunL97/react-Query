* 클아이언트상태: 웹브라우저 세션과 관련된 모든 정보를 의미
* 서버상태: 서버에저장되지만 클라이언트에 표시하는데 필요한데이터
* 리액트쿼리: 클라이언트에서 서버데이터 캐시를관리, 데이터관리뿐만아니라 서버상태관리에 도움이 되는 많은 도구가 함께제공
* 서버에대한 모든 쿼리의 로딩 및 오류상태를 유지해주기 때문에 수동으로 할 필요가 없어짐
* 사용자를 위해 데이터의 패이지네이션이나 무한스크롤도 제공
* QueryClient: 클라이언트가 있기때문에 공급자를 추가가능, 그러면 공급자가 클라이언트를 소품으로 사용하게 되면서 클라이언트가 가지고 있는 캐시와 모든 기본옵션을 공급자의 자녀 컴포넌트도 사용가능
* isLoading: 데이터가 가져와졌는지 안가져와졌는지 확인가능
* isFetching: 비동기 쿼리가 해결되지 않았음을 의미 ex) Axios 호출 
* isLoading은 isFetching안에 포함되어 있다
* isError: 에러가났는지 안났는지 알려준다
* error: 무슨 에러가 났는지 알려준다
* staletime: 데이터를 허용하는 최대나이, 데이터가 만료됐다고 판단하기전까지 허용하는 시간 ex) 웹사이트에 표시된 데이터가 10초까지는 그대로여도 괜찮으면 stabletim을 10초로 설정, 리페칭할때 고려사항, 기본은 0으로설정되어있음
* chchetime: 나중에 다시 필요할 수 도있는 데이터용, 기본은 5분으로 설정되어있음, 5분이 지나면 캐시의 데이터가 만료된다., 페이지에 표시되는 컴포넌트가 특정 쿼리에 대해 useQuery를 사용하는 시간을 말하는것, 캐시가 만료되면 가비지컬렉션이 실행되고 클라이언트는 데이터를 사용할 수 없음, 데이터가 캐시에 있는 동안에는 fetching할때 사용될수 있음, 데이터 페칭을 중지하지 않기때문에 서버의 최신데이터로 새로고침이 된다. 근데 계속해서 빈페이지만 보는경우가 생길 수 있는데 이때는 오래된 데이터를 표시하는편이 빈 페이지보다 낫다.
* 가비지컬렉션: 더이상 사용되지 않는 동적으로 할당된 메모리공간을 자동으로 해제하는 프로세스, 이를통해서 메모리릭을 방지하고 메모리관리를 효율적으로할 수 있음. 현대적인 프로그래밍언어에서 자동으로 수행되므로 개발자가 직접 메모리 관리를 처리할 필요가 없어져서 프로그래밍 생산성과 안정성을 향상시킬 수 있음
*  쿼리키를 쿼리에 대한 의존성 배열로 취급가능, 쿼리키가 변경되면 ['comments', post.id] 즉 post.id가 업데이트 되면 React Query가 새 쿼리를 생성
*  예를들어 블로그 글의 댓글을 불러올 때 의존성 키를 넣지 않으면 계속 처음게 나옴, 업데이트 되기때문
*  페이지네이션: 페이지마다 다른 쿼리키가 필요 -> 쿼리키를 배열로 업데이트해서 가져오는 페이지 번호를 포함하면된다.
* prefetching: 데이터를 캐시에 추가하며 구성할 수 있긴 하지만 기본값으로 만료상태 -> 데이터를 사용하고자할 때 만료상태에서 데이터를 다시가져옴 -> 추후 사용자가 사용할 법한 모든 데이터에 프리페칭을 사용
* 프리패칭은 useQuery라는 훅을 통해 queryclient를 통해서 가져올 수 있음 
```
queryClient.prefetchQuery(["posts", nextpage], () => 함수,
{
    stableTime: 2000,
    keeyPreviousData: true, // 이전페이지로 돌아갔을때 캐시에 해당데이터있도록 설정
});
```
* isFetching: async 쿼리 함수가 해결되지 않았을 때 참 -> 아직데이터를가져오는중
* isLoading: isFetching이 참이면서 쿼리에 대해 캐시된 데이터가 없는 상태
* isLoading은 캐시된 데이터가 없고 데이터를 가져오는 상황에 해당하는 isFetching의 부분집합 -> 처음실행된 쿼리일때 로딩여부를따짐
* mutation: 서버에 데이터를 업데이트하도록 서버에 네트워크 호출을 실시-> 데이터를 추가하거나 삭제
* mutation은 데이터를 저장하지 않으므로 쿼리키가 필요하지않음
* 캐시된 항목이 없으면 isFetching은 성립x
* useQuery는 기본값으로 3회 재시도
* useMuation은 기본값으로는 재시도를하지 않지만 자동 재시도를 적용하고 싶다면 설정가능