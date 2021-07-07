---
description: React 컴포넌트 간에 코드를 공유하기 위해 함수 props를 이용하는 간단한 테크닉
---

# Render Props

🔍 **참고**  
- [Render Props](https://ko.reactjs.org/docs/render-props.html)

### 예시 코드

부모에게서 요소 받아 렌더링 시킬   children을 사용하는 대신 Render Props 사용하는 방법

```text
import React, { ReactElement } from 'react';
import styled from 'styled-components';

interface DefualtProps {
  title?: string;
  subTitleRender?: () => JSX.Element;
}

const WrapEl = styled.div`
  display: flex;
  align-items: center;
  justify-content: space-between;
`;

const Title = ({ title, subTitleRender }: DefualtProps): ReactElement => (
  <WrapEl>
    {title && <h1>{title}</h1>}
    {subTitleRender && <div>{subTitleRender()}</div>}
  </WrapEl>
);

export default Title;

```

#### Title 컴포넌트 사용 예시

```text
import Title from './Title';

<Title title="타이틀" subTitleRender={() => <stong>서브 타이</stong>} />
```

