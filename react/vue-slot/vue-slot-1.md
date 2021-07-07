---
description: 참고 블로그 포스팅을 보고 작업한 예시 코드
---

# 리액트에서 Vue의 slot 기능 사용 예시1

### 예시 코드

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
  const elements = Array.isArray(children) ? children : new Array(children);
  const subTitle = elements.find((element: { type: { name: string } }) => element?.type.name === 'SubTitle');

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

