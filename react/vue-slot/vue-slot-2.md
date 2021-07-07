---
description: React.Children.toArray() 사용
---

# 리액트에서 Vue의 slot 기능 사용 예시2

🔍 **참고**  
- [React.Children.toArray](https://ko.reactjs.org/docs/react-api.html#reactchildrentoarray)

### 수정 사항

#### 1. React.Children.toArray\(\) 사용

* 각 자식에 `key`가 할당된 배열을 `children`  자료 구조로 반환.

```text
  const elements = Array.isArray(children) ? children : new Array(children);
  
  const elements = React.Children.toArray(children);
```

#### 2. 

* 음...  `element: { type: { name: string } }`  빨간줄이 계속가는데 방법을 모르겠다..  `any` 로 우선

```text
    const subTitle = elements.find((element: { type: { name: string } }) => element?.type.name === 'SubTitle');
    
    const subTitle = elements.find((element: any) => element.type.name === 'SubTitle');
```



### 수정된 예시 코드

```text
import React, { ReactElement } from 'react';
import styled from 'styled-components';

interface DefaultProps {
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  children?: any;
  title?: string;
}

const WrapElement = styled.div`
  display: flex;
  align-items: center;
  justify-content: space-between;
`;

interface ElProps {
  children?: React.ReactNode;
}
const SubTitle= ({ children }: ElProps): ReactElement => <stong>{children}</stong>;

const Title = ({ title, children }: DefaultProps): ReactElement => {
  const elements = React.Children.toArray(children);
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  const subTitle = elements.find((element: any) => element.type.name === 'SubTitle');

  return (
    <WrapElement>
      {title && <h1>{title}</h1>}
      {subTitle && subTitle.props.children}
    </WrapElement>
  );
};

Title.SubTitle = SubTitle;

export default Title;

```

#### Title 컴포넌트 사용 예시

```text
import Title from './Title';

<Title title="타이틀">
  <Title.subTitle>서브 타이틀</Title.SubTitle>
</Title>
```



