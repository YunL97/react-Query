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
* prefetchQuery: 미리 데이터를 가져와서 캐시에 저장해두는기능: 데이터가 필요할 때 즉시 사용할 수 있어서 네트워크 지연 시간을 줄일 수 있다.
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
* 리액트쿼리는 반환되는 객체에 refetch 함수가 포함되어있다. 또는 자동 리패치를 수행도 가능리액트 쿼리는 알려진 키에 대해서만 새 데이터를 가져온
* 페이지가 바뀔때마다 데이터가 변경될 경우 키도 변경되도록 해야함 -> 새 쿼리를 만들고 새 데이터를 가져오기 위해
* keepPreviousData: true = 쿼리키가 변경될때 까지 이전의 모든 데이터가 그대로 유지
* 페이지네이션 사용해서 데이터 가져올때  다음 페이지 데이터를 미리 프리패칭 하려면 useeffect 사용해서 하면될듯
  
* useQuery의 셀렉트 옵션: 쿼리함수가 반환하는 데이터를 변환 가능
* select: 함수 넣어서 걸러낸다
* 리액트에서의 최적화: 데이터의 변경여부와 함수의 변경여부를 확인하고 변경사항이 없으면 해당 함수를 다시 실행하지 않는것 => 즉 불필요한 리렌더링을 하지않는것
* 훅에 함수가 있으면 훅이 실행될때마다 변경되는데 그럼 리랜더링이 실행되기 때문에 useCallback를 사요하면 좋음
*  stale 쿼리는 어떤 조건 에서 자동적으로 다시가져오기를한다 
   *   새로운 쿼리인스턴스가 많아아지거나 
   *   쿼리키가 처음 호출 되거나 
   *   쿼리를 호출하는 반응 컴포넌트를 증가시킨다거나 
   *   창을 재포커스 할때나 
   *   만료된 데이터의 업데이트 여부를 확인할수 있는 네트워크가 다시연결될경우

* 리패칭 제한하는법:  리패칭을 제한할때는 신중해야된다.-> 미세한 변동에도 큰변화를 불러오는 데이터에는 적용하면 안된다
  * stale시간 증가 -> 창을 재포커스 하거나 네트워크에 재연결하는 트리거는 데이터가 실제로 만료된 경우에만 작용
  * 불리언 옵션 끄면됨 
  * refetchOnMount: 컴포넌트가 마운트될때 쿼리가 자동으로 가져온다 이옵션은 일반적으로 데이터를 로드하려는 경우 사용
  * refetchOnWindowFocus: 브라우저 창이 포커스를 받았을 때 쿼리가 자동으로 다시가져온다.
  * refetchOnReconnect: 오프라인에서 온라인으로 전환될 때 자동으로 쿼리가 다시 가져온다. 사용자가 인터넷에 연결할 때 데이터가 최신 상태인지 확인하려는 경우에 사용
  * 이런거를 false로 놓는다는것은 보수적이라는뜻 -> 네트워크 호출에 보수적, 대부분의 쿼리에서 리페칭을 할만큼 데이터 변경이 충분하지 않다는 뜻
* 데이터가 업데이트 되지 않는것보다 항상 실시간인것 보다 낫다
* catcheTime 을 staleTime보다 작게 설정하면 캐시의 쓸모가 없어짐
* 
* refetchInterval: 시간설정해두면 그 시간마다 데이터를 가져옴

* 인증된 앱은 Auth 로 React query를 활성화 하는것
* 인증 부분은 유동적인 조각이 많다
* JWT: Json Web Token, 클라이언트가 사용자명과 비밀번호를 보내고 이사용자명과 비밀번호가 데이터베이스에서 일치한다면 서버가 토큰을 보내는 방식으로 작동 -> 언제든 클라이언트가 필요로 하면 로그인이 필요한 서버에서 클라이언트는 헤더의 토큰을 요청과 함께 보낸다. -> 서버가 이 클라이언트가 인증됐음을 인지
* 보안방법: 서버가 토큰을 생성하고 사용자명, 사용자 id와 같은 정보를 인코딩, 인코딩된 토큰이 서버로 돌려보내지면 디코딩이 되고 데이터가 일치함을 확인(그래서 서버를 설치할때 암호가 필요) -> 토큰을 인코딩, 디코딩하는데 암호가 사용되는것
* 리액트쿼리의 책임: 클라이언트의 서버 상태를 관리하는것
* 데이터 저장은 리액트쿼리에 하는게 맞음

* 리액트 쿼리는 쿼리캐시데이터를 브라우저에 유지하는 플러그인을 제공
* 데이터 앞에 ! 를 붙이면 불리언타입이 된다
* 리액트쿼리의 장점: 새로운 데이터가 있을 때 새 데이터를 위해 서버에 핑을 실행하기 보다 캐시에서데이터를 가져온다는것
* 데이터를 지울때는 removeQuery를 사용하지 않고 setqueryData에 null을 사용하는게 좋ㅇ음 -> 비동기이기 때문인듯
  
* useMutation
  * 일회성이기 때문에 캐시 데이터가 없다. 페칭이나 리페칭 그리고 업데이트할 데이터가 있는 usequery와 다른점
  * 기본적으로 재시도가 없음, useQuery는 기본적으로 세번 재시도
  * 관련된 데이터가 없으므로 리페치도 없다
  * 캐시 데이터가 없으므로 isLoiding, isFetching이 구분되지 않는다
  * 캐시데이터 개념이 없으므로 isFetching만 있다.
  * 쿼리키가 필요없음
  * 변이함수만 있으면 됨

* 쿼리키 접두사: 동일한 쿼리키 접두사로 서로 관련된 쿼리를 설정하면 모든 쿼리를 한번에 무효화 가능
* queryClient.invalidateQueries: 특정 쿼리 또는 쿼리들을 갱신하기 위해 사용 캐시된 데이터를 갱신하기위해 호출
* queryClient.removeQueries('user'): user라는 키를 가진 모든 쿼리가 캐시에서 제거
* mutation을 이용해서 서버에서 받은 데이터로 데이터도 업데이트가능
* 낙관적 업데이트: 서버로부터 부터 응답을 받기전에 사용자 캐시를 업데이트하는것 
  *  장점: 캐시가 더 빨리 업데이트 된다는것
  *  단점: 서버 업데이트가 실패한 경우 코드가 더 복잡해진다 -> 업데이트 이전의 데이터로 되돌려야한다 -> 해당 데이터를 저장해줘야한다는뜻
  *  오래된 서버 데이터는 낙관적 업데이트를 덮어쓰지 말아야 한다
  *  쿼리 취소 메소드를 사용해서 발신 쿼리를 취소할 수 있다 ex) queryClient.cancelQueries(queryKey.user)
  *  그냥 서버 통신 했는데 실패했을때 이전데이터를 가져온다 이런 느낌인듯?
  *  동일한 데이터를 사용하는 컴포넌트가 다수 있으며 서버에서 업데이트가 조금 오래 걸릴경우 아주 강력하며 사용자 측에서는 웹사이트가 훨씬 반응성이 좋게 느껴진다.

*  다시한번 상기하자면 리액트쿼리는 서버와 클라이언트 간의 커뮤니케이션인 네트워크 호출과 주로 연관이 되어있다
*  MSW: 네트워크 호출을 조정하여 테스트 조건을 설정하는데 도움을 주는 라이브러리

```
test('renders response from query', () => {
  render(<Treatments />)
})
```
* 병렬로 실행하는경우 테스트의 신뢰도가 낮아질 수 있다.
* test-utils에 index.tsx에서 queryclient를 설정해줘야함
```
const generateQueryClient = () => {
  return new QueryClient()
}

export function renderWithQueryClient(
  ui: ReactElememt,
  client?: QueryClient
): RenderResult {
  const queryClient = client ?? genenateQueryClient()

  return render(<QueryClientProvider client={queryClient}>{ui}</queryClient>)
}
```