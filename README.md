# useQuery-data-after-setState

## react-query 로 데이터를 불러 왔을 때 후 처리를 해야하는 스테이트를 만들어야 하는경우가 있습니다 변환 방법 은 여러가지가 있는데,

### 1. onSuccess 를 활용하는 방법

query 호출 시 options 로 유동적이게 사용

### 2. select 를 사용하는 방법

```ts
    return useQuery(queryKey, callBack, {
        onError: (error?: unknown) => onErrorFun(error, dispatch , navigate), // error 발생 할 경우 타는 function
        select: (data: any) => data?.data?.response, // data result 변환처리
        retry: 0, // 실패시 재 요청 횟수
        enabled: enabled !== undefined ? enabled : true,
        refetchOnWindowFocus: false,
        refetchOnMount: false,
        refetchOnReconnect: false,
    });
```

### 3. state 를 변경해야할 때 사용하는 방법

```ts
   useEffect( () => {
       console.log(selectMemberListQuery)
       if (selectMemberListQuery.status === "success")
           setMemberList(selectMemberListQuery?.data?.list?.map( (data : ListElementProps, index: number) => {
                        return {
                            ...data,
                            checked: allCheckBoxState
                        }

                    }))
    } , [selectMemberListQuery.status , allCheckBoxState , selectMemberListQuery.isFetching])

```

### 4. useMemo 사용하기

```ts
    // const checkedValueList = useMemo( () => {
    //     return selectMemberListQuery?.data?.list?.map( (data : ListElementProps, index: number) => {
    //         return {
    //             ...data,
    //             checked: allCheckBoxState
    //         }
    //
    //     })
    // }, [selectMemberListQuery , allCheckBoxState])

```
