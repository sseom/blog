---
description: 'React.cloneElement(), React.isValidElement(object) 사용'
---

# 리액트에서 Vue의 slot 기능 사용 예시3

🔍 **참고**  
- [React.cloneElement](https://ko.reactjs.org/docs/react-api.html#cloneelement)  
- [React.isValidElement\(object\)](https://ko.reactjs.org/docs/react-api.html#isvalidelement)

### 수정 사항

#### 1.  React.cloneElement\(\) 사

* `element`를 기준으로 새로운 React 엘리먼트를 복사하고 반환합니다.

```text
{subTitle && subTitle.props.children}
  
{subTitle && React.cloneElement(subTitle as React.ReactElement)}
```

#### 2. React.isValidElement\(object\) 사용

*  객체가 React 엘리먼트인지 확인합니다. `true` 또는 `false`를 반환

```text
{subTitle && subTitle.props.children}
  
{React.isValidElement(subTitle) && React.cloneElement(subTitle)}
```



### 수정된 예시 코드

```text
import React, { ReactElement } from 'react';
import styled from 'styled-components';

interface DefaultProps {
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
  const subTitle = elements.find((element: any) => element.type.name === 'SubTitle');

  return (
    <WrapElement>
      {title && <h1>{title}</h1>}
      {/* {subTitle && React.cloneElement(subTitle as React.ReactElement)} */}
      {React.isValidElement(subTitle) && React.cloneElement(subTitle)}
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





