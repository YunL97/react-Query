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
* staletime: 데이터를 허용하는 최대나이, 데이터가 만료됐다고 판단하기전까지 허용하는 시간 ex) 웹사이트에 표시된 데이터가 10초까지는 그대로여도 괜찮으 staletime을 10초로 설정, 리페칭할때 고려사항, 기본은 0으로설정되어있음
* chchetime: 나중에 다시 필요할 수 도있는 데이터용, 기본은 5분으로 설정되어있음, 5분이 지나면 캐시의 데이터가 만료된다., 페이지에 표시되는 컴포넌트가 특정 쿼리에 대해 useQuery를 사용하는 시간을 말하는것, 캐시가 만료되면 가비지컬렉션이 실행되고 클라이언트는 데이터를 사용할 수 없음, 데이터가 캐시에 있는 동안에는 fetching할때 사용될수 있음, 데이터 페칭을 중지하지 않기때문에 서버의 최신데이터로 새로고침이 된다. 근데 계속해서 빈페이지만 보는경우가 생길 수 있는데 이때는 오래된 데이터를 표시하는편이 빈 페이지보다 낫다.
* 가비지컬렉션: 더이상 사용되지 않는 동적으로 할당된 메모리공간을 자동으로 해제하는 프로세스, 이를통해서 메모리릭을 방지하고 메모리관리를 효율적으로할 수 있음. 현대적인 프로그래밍언어에서 자동으로 수행되므로 개발자가 직접 메모리 관리를 처리할 필요가 없어져서 프로그래밍 생산성과 안정성을 향상시킬 수 있음
*  쿼리키를 쿼리에 대한 의존성 배열로 취급가능, 쿼리키가 변경되면 ['comments', post.id] 즉 post.id가 업데이트 되면 React Query가 새 쿼리를 생성
*  예를들어 블로그 글의 댓글을 불러올 때 의존성 키를 넣지 않으면 계속 처음게 나옴, 업데이트 되기때문
*  페이지네이션: 페이지마다 다른 쿼리키가 필요 -> 쿼리키를 배열로 업데이트해서 가져오는 페이지 번호를 포함하면된다.
* prefetching: 데이터를 캐시에 추가하며 구성할 수 있긴 하지만 기본값으로 만료상태 -> 데이터를 사용하고자할 때 만료상태에서 데이터를 다시가져옴 -> 추후 사용자가 사용할 법한 모든 데이터에 프리페칭을 사용 , 페이지네이션 다음데이터를 미리가져옴
* 프리패칭은 useQuery라는 훅을 통해 queryclient를 통해서 가져올 수 있음 
```
queryClient.prefetchQuery(["posts", nextpage], () => 함수,
{
    staleTime: 2000,
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
* 리액트쿼리 개발자도구는 노드 환경을 변경할 수 있을때만 보이게된다, 노드환경이 프로덕션으로 설정되어있지 않은경우
* pageParams는 많이 사용되지 않는다 모든 쿼리는 페이지 배열에 고유한 요소를 가지고 있고 그 요소는 해당쿼리에 대한 데이터에 해당
* infiniteQuery에서 pageParam은 fetchNextPage가 어떻게 보일지 결정하고 다음페이지가 있는지 결정
* 무한스크롤에서 스크롤이 원위치 되는데 새로운 페이지를 열어야 할 때 조기 반환이 실행되기 때문
* isLoading을 isFetching로 바꾸면 된다 -> isFetching은 조기반환을 하지 않기때문
* 조기반환(early return): 렌더링 도중 조건문등의 검사를 해서 특정조건이 만족되지 않으면 함수의 실행을 중단하고 빈값을 반환하는것
* 양방향스크롤: 데이터의 중간부터 시작할때 유용 useInfiniteQuery는 양방향스크롤도 고려해서 만들어졌기 때문에 유용하게 사용가능
* 커스텀훅 장점
  * 다수의 컴포넌트에서 데이터를 엑세스 해야하는경우 useQuery호출을 재작성할 필요가 없음
  * 다수의 useQuery호출을 사용했다면 사용중인 키의 종류가 헷갈릴수 있는데 커스텀 훅을 사용해 매번 호출한다면 키를 헷갈릴 위험이 없다
  * 사용하길 원하는 쿼리 함수를 혼동하는 위험도 없다'
  * 커스텀훅에 넣어주면 다수의 컴포넌트에 굳이 불러올 필요가 없다

* 쿼리키는 리액트쿼리의 작업을 쵣한 효율적이 되도록 하는키
* isLoading = isFetching - 캐시데이터
* useIsFetching: 현재 가져오는 중인 쿼리가 있는지를 알려주는 훅, useIsFettching이 0보다 크면 가져오는 데이터가 있다는뜻
```
const isFetching = useIsFetching();

const display = isFetching ? 'inherit' : 'none'; 
```
* 리액트쿼리는 자동으로 윈도우에 다시돌아올때 데이터를 다시가져온다
* 에러핸들링응 중앙화하는법: onError 에서 나오는 error을 커스텀 훅으로 보내면 될듯? 아니면 훅대신 function
* 에러핸들링응 중앙화하는법: onError 에서 나오는 error을 커스텀 훅으로 보내면 될듯?
*  데이터를 미리 가져올때 onclick넣는 것은 좋은방법이 아님 useEffect 의존성으로 설정해서 페이지가 변경될때마다 함수실행하게 하는게 좋음
```
useEffect(() => {
    const nextpage = currentpage + 1;
    queryClient.prefetchQuery(["posts", nextPage], () => fetchPosts(netPage))
}, [currentPage, queryClient])
```
*  리액트쿼리가 useError훅을 사용하지 않는이유: 깊이 생각해보면 useError훅이 존재 할수없음  -> 정수 이상의 값이 반환되어야 하기때문 -> 사용자에게 오류를 표시하려면 각 오류에 대한 문자열이 필요한데 각기 다른 문자열을 가진 오류가 시시각각 팝업 하고록 구현하는 것은 쉽지 않음
*  QueryClient를 위해서 onError 핸들러 기본값을 만드는게 좋다
* 커스텀훅 ex
```
interface UseStaff {
   staff: Staff[];
   filter: strig;
   setFilter: Dispatch<SerStateAction<String>>;

   export function useStaff(): UseStaff {
      const [filter, setFilter] = useState('all');

      const fallback = [];
      const { data: staff = fallback} = useQuery(queryKey.staff, getStaff); 


      return {staff, filter, setFilter};
   }
}
```
* React Query로 데이터를 미리 채우는 방법이있음 -> 일반적으로 사용자에게 보여주고 싶은 정보가 있을때 캐시에 아직 데이터가 없는 경우 미리 데이터를 채울 수 있음
  * 1. prefetchQuery: 데이터를 서버로부터 가져옴, ,method to queryClient 캐시를 더할수있나 0
  * 2. setQueryData :데이터를 클라이언트로부터가져옴 ,method to queryClient 캐시를 더할수있나 0
  * 3. placeholderData: 데이터를 클라이언트로부터 가져옴, ,method to useQuery 캐시를 더할수있나 x
  * 4. initialData: 데이터를 클라이언트로부터 가져옴, method to useQuery 캐시를 더할수있나 0

* 데이터를 미      리 가져오는 것은 동적인 데이터 (ex주식) 는 부적합 하다
* useQuery는 페칭과 리페칭등의 작업이 필요한 ㅝ리를 생성
* prefetchQuery 일회성-> 쿼리클라이언트 메소드이므로 쿼리클라이언트를 반환해야하면 이를 위해 useQuery클라이언트 훅을 사용해야한다
```
export function useA(): Treatment[] {
   conat fallback =[];
   const {data = fallback} =useQuery(queryKeys.trements, getTrements);
   return data;
}

export function usePrefetchA(): void { //캐시만 채우는게 목적이므로 리턴 없어도 됨
   const queryClient = useQueryClient();
   queryColent.prefetchQuery(queryKeys.trements, getTrements);
}
```
* 프리페치는 탑레벨에서 실행시키는건가?
* 훅이 아닌경우 훅내에서 useQueryClient 사용불가능
* 