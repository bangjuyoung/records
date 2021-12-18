---
description: https://www.apollographql.com/docs/react/
---

# Apollo Client

`GraphQL` 을 사용하여 로컬과 원격 데이터를 관리할 수 있는 상태 관리 라이브러리 &#x20;

데이터를 페치, 캐시, 수정할 때 사용한다.

&#x20;데이터 변경시 자동으로 UI 업데이트 한다.

핵심 라이브러리`@apollo/client에 React가 접목되어 있다.`

### 기본 사용 방법

ref. [https://www.apollographql.com/docs/react/get-started/](https://www.apollographql.com/docs/react/get-started/)

#### 1. 설치

`npm install @apollo/client graphql`

* `@apollo/client`: `Apollo Client` 세팅시 필요한 패키지로, in-memory 캐시, 로컬 `상태 관리, 에러 처리, 리액트 컴포넌트를 포함한다.`
* `graphql`: GraphQL 쿼리 파싱을 담당하는 패키지

#### 2. [`ApolloClient`](https://www.apollographql.com/docs/react/get-started/#2-initialize-apolloclient) `초기화`

**`2-1.`필요한 모듈 추가**&#x20;

```js
import {
  ApolloClient,
  InMemoryCache,
  ApolloProvider,
  useQuery,
  gql
} from "@apollo/client";
```

**2-2. ApolloClient 초기화**

> `uri`, `cache` **** 속성을 가진 **** 설정 객체를 전달한다.

```js
const client = new ApolloClient({
  uri: 'https://48p1r2roz4.sse.codesandbox.io',
  cache: new InMemoryCache()
});
```

* `uri` GraphQL 서버 `URL`
* `cache` `InMemoryCache 인스턴스, Apollo Client`가 `쿼리 페치 후 결과를 캐싱하기 위해` 사용

**참고: 자바스크립트 사용 예시**

```js
client
  .query({
    query: gql`
      query GetRates {
        rates(currency: "USD") {
          currency
        }
      }
    `
  })
  // 응답에 data 속성 외, loading 와 neworkStatus 속성 확인 가능
  .then(result => console.log(result));
```

#### 3. 리액트 연동하기

> 쿼리를 연동하여 데이터가 변경되면 자동으로 UI를 업데이트 할 수 있다.

`ApolloProvider`(리액트 `Context.Provider` 와 유사) 컴포넌트로 리액트 app를 감싸 `Apollo Client Context` 안에 둔다.

&#x20;`GraphQL data` 가 필요한 컴포넌트 최 상위에 두어 어디서든 접근할 수 있게 한다.

```jsx
import React from 'react';
import { render } from 'react-dom';
import {
  ApolloClient,
  InMemoryCache,
  ApolloProvider,
  useQuery,
  gql
} from "@apollo/client";

const client = new ApolloClient({
  uri: 'https://48p1r2roz4.sse.codesandbox.io',
  cache: new InMemoryCache()
});

function App() {
  return (
    <div>
      <h2>My first Apollo app 🚀</h2>
    </div>
  );
}

render(
  <ApolloProvider client={client}>    <App />  </ApolloProvider>,  document.getElementById('root'),
);
```

**4. useQuery 를 사용하여 데이터 가져오기**

> `useQuery`:  UI 와 GraphQL 데이터를 공유하는 리액트 훅

```js
// query 정
const EXCHANGE_RATES = gql`
  query GetExchangeRates {
    rates(currency: "USD") {
      currency
      rate
    }
  }
`;
```

```js
// useQuery 로 데이터 가져오기
// 컴포넌트가 렌더될 때마다 useQuery가 실행되고 데이터에 따라 UI가 자동으로 업데이트된다.
// 응답 객체는 loading, error 상태와 쿼리 결과 값인 data 속성을 가진다.
function ExchangeRates() {
  const { loading, error, data } = useQuery(EXCHANGE_RATES);
  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error :(</p>;

  return data.rates.map(({ currency, rate }) => (
    <div key={currency}>
      <p>
        {currency}: {rate}
      </p>
    </div>
  ));
}
```
