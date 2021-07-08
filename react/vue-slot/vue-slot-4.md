---
description: 가장 바보 같았던.. 내용 일수도.. ㅋㅋㅋ
---

# 리액트에서 Vue의 slot 기능 사용 예시4

### 생각의 오류

* [리액트에서 Vue의 slot 기능](https://sseom.gitbook.io/blog/react/vue-slot) 사용에서 작성한 것 처럼  해당 컴포넌트 사용시에 children 영역에 `컴포명.SubTitle`, `컴포명.Content` 요소를 반복적으로 사용하면  당연히 반복 되서 화면이 나올 줄 알았다.. 😂 아니지 아니지 아니였지!
* 내가 하고 싶은건  타이틀1 콘텐츠1   타이틀2  콘텐츠2   이렇 순서대로 나열됐음 좋겠다.



### 해결 방법

#### 1. elements 확

children 영역에 원하는대로 다 넣어보고 elements를 콘솔로 찍어보자   
배열이 나오고 배열 안에는 children 영역에 넣은 요소들이 객체로 들어가 있다.

```text
const elements = React.Children.toArray(children);
console.log(elements); // [{…}, {…}, {…}, {…}] 배열 안에 객체들로 나온다.
```

#### 2. subTitle 확

subTitle을 콘솔로 찍어보자  
리액트 엘레먼트 객체가 나오는데  문제는 자식이 영역에 넣은 여러가지중 딱 한개만 나온다는 것!  
당연하지!!  .find\(\) 를 사용했으니 조건에 맞는 값 중 첫번째 값을 리턴 하니까..

그렇다면?   
조건에 맞는 배열의 모든 값을 배열 형태로 리턴 하는 .filter\(\) 를 사용해볼까?  
자식영역에 넣은 subTitle 들이 들어가 있는 배열이 나온다. 🤩

참고  
- [Array.prototype.find\(\)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)  
- [배열의 특정값 찾기 find\(\), filter\(\)](https://hianna.tistory.com/406)

```text
const subTitle = elements.find((element: any) => element.type.name === 'SubTitle');
console.log(subTitle); // {$$typeof: Symbol(react.element)....}

const subTitle = elements.filter((element: any) => element.type.name === 'SubTitle');
console.log(subTitle); // [{…}, {…}]
```

어떻게 나오는지 봐볼까?  
😭자식영역에 넣은 서브타이틀 콘텐츠 다 나오긴 하는데...  
서브타이틀은 서브타이틀 대로  콘텐츠는 콘텐츠 순으로 나온다.

```text
{React.isValidElement(subTitle) && React.cloneElement(subTitle)}
{React.isValidElement(Content) && React.cloneElement(Content)}
```

#### 3. 멍청했고,, 다시 elements 확인... 방법 찾기 

멍청했지 멍청했어!  
.filter\(\) 사용해서 조건에 맞는 값을 배열로 받아서 이것을 뿌리니.. 당연하지.  
여기선 .find\(\), .filter\(\) 필요 없어.  
  
처음에 확인했던 elements를 보자   
내가 작성한 모든 자식요소들이 들어가 있다. 이걸 뿌리면 되는거 아닌가??  
  
배열에 들어간 요소들을 한개씩 꺼내서 뿌리는 방법 음.. 🤔  
.map\(\)을 사용해볼까??!!   


#### 4. map\(\) 사용 

map\(\)은 배열 요소마다 주어진 함수\(콜백\)를 호출한 결과를 모아 **새로운 배열**을 반환.  
그리고 기본 배열은 순수하게 유지됨.

```text
const elements = React.Children.toArray(children);
const childrenElements = elements.map(children => React.isValidElement(children) && React.cloneElement(children));

return (
    <section>
      {title && <h1>{title}</h1>}
      {childrenElements && childrenElements}
    </section>
);
```

#### 5. forEach\(\) 사용 

forEach\(\)가 배열 요소마다 한 번씩 주어진 함수\(콜백\)를 실행하고 기존 배열을 변경.  
그리고 반환값은 없음.

```text
const elements = React.Children.toArray(children);
elements.forEach(children => React.isValidElement(children) && React.cloneElement(children));

return (
    <section>
      {title && <h1>{title}</h1>}
      {elements && elements}
    </section>
);
```

#### 6. 수 코드 

```text
import React, { ReactElement } from 'react';
import styled from 'styled-components';

interface DefaultProps {
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  children?: any;
  title?: string;
}

interface ElProps {
  children?: React.ReactNode;
}
const SubTitle= ({ children }: ElProps): ReactElement => <stong>{children}</stong>;
const Content= ({ children }: ElProps): ReactElement => <div>{children}</div>;

const Title = ({ title, children }: DefaultProps): ReactElement => {
  const elements = React.Children.toArray(children);
  elements.forEach(children => React.isValidElement(children) && React.cloneElement(children));

  return (
    <section>
      {title && <h1>{title}</h1>}
      {elements && elements}
    </section>
  );
};

Section.SubTitle = SubTitle;
Section.Content = Content;

export default Section;

```



### 근데 모든게 다 바보같은 짓이였다.  그냥 { children }  만 있음 돼  😭

### 최종 코드

```text
import React, { ReactElement } from 'react';
import styled from 'styled-components';

interface DefaultProps {
  children?: React.ReactNode;
  title?: string;
}

interface ElProps {
  children?: React.ReactNode;
}
const SubTitle= ({ children }: ElProps): ReactElement => <stong>{children}</stong>;
const Content= ({ children }: ElProps): ReactElement => <div>{children}</div>;

const Title = ({ title, children }: DefaultProps): ReactElement => (
  <section>
    {title && <h1>{title}</h1>}
    {children}
  </section>
);

Section.SubTitle = SubTitle;
Section.Content = Content;

export default Section;

```

#### 

### Section 컴포넌트 사용 예시

```text
import Section from './Section';

<Section title="섹션 타이틀">
  <Title.subTitle>서브 타이틀 1</Title.SubTitle>
  <Title.Content>
    <p>콘텐츠 1</p>
  </Title.Content>
  
  <Title.subTitle>서브 타이틀 2</Title.SubTitle>
  <Title.Content>
    <p>콘텐츠 2</p>
  </Title.Content>
  
  <Title.subTitle>서브 타이틀 3</Title.SubTitle>
  <Title.Content>
    <p>콘텐츠 3-1</p>
    <div>콘텐츠 3-2</div>
  </Title.Content>
</Section>
```



