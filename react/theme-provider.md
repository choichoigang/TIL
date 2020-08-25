# styled-component ThemeProvider

매번 프로젝트를 진행하면서 코드의 양이 증가함에 따라서 스타일 작업에 관한 코드들의 일관성이 확연하게 떨어지고 불필요한 코드와 전체적인 스타일 규격이 맞지 않는 경험을 했습니다. 이는 팀원과 협업을 하는 과정에서도 추후에 문제가 커질 것이라고 생각이 들었고 Styled-components의 ThemeProvider를 사용해서 여러 프로젝트에서 재사용할 수 있는 나만의 Theme Style을 구축하는 경험을 공유하고자 합니다.

- 문제점

1. 스타일의 일관성이 떨어진다.
2. 협업의 관점에서 봤을 때 Style Convention이 크게 떨어진다.
3. 정해진 규격이 없어서 스타일 작업에 대한 시간이 증가한다.

# ThemeProvider

ThemeProvider의 작동 방식은 Context API를 기반으로 이루어져 있다. ThemeProvider로 감싸진 자식 Component들은 ThemeProvider로 전달받은 theme를 props로 전달받아서 사용이 가능합니다.

```
import React from "react";
import { ThemeProvider } from "styled-components";

const App = () => {
  return (
    <div>
      <ThemeProvider theme={theme}>
        <Navbar />
        <Search />
        <IntroBooks />
      </ThemeProvider>
    </div>
  );
};
```

위 코드의 예시 같은 경우에는 App.js에서 Rendering 되고 있는 하위 Component들의 밖에 ThemeProvider를 선언해 주었고 하위에 있는 Component들은 전달받은 theme의 값들을 받아서 사용하고 있습니다.. 전달할 theme의 경우에는 ThemeProvider의 theme props로 내가 지정한 theme의 값들을 전달해 주면 됩니다.

# theme.js 구현

일단 하위 Component에게 전달할 theme.js에 Style 요소들을 지정해 주는 작업을 하겠습니다.

```
const calcRem = (size) => `${size / 16}rem`;

const fontSizes = {
  small: calcRem(14),
  base: calcRem(16),
  lg: calcRem(18),
  xl: calcRem(20),
  xxl: calcRem(22),
  xxxl: calcRem(24),
  titleSize: calcRem(50),
};

const paddings = {
  small: calcRem(8),
  base: calcRem(10),
  lg: calcRem(12),
  xl: calcRem(14),
  xxl: calcRem(16),
  xxxl: calcRem(18),
};

const margins = {
  small: calcRem(8),
  base: calcRem(10),
  lg: calcRem(12),
  xl: calcRem(14),
  xxl: calcRem(16),
  xxxl: calcRem(18),
};

const interval = {
  base: calcRem(50),
  lg: calcRem(100),
  xl: calcRem(150),
  xxl: calcRem(200),
};

const verticalInterval = {
  base: `${calcRem(10)} 0 ${calcRem(10)} 0`,
};

const deviceSizes = {
  mobileS: "320px",
  mobileM: "375px",
  mobileL: "450px",
  tablet: "768px",
  tabletL: "1024px",
};

const colors = {
  black: "#000000",
  white: "#FFFFFF",
  gray_1: "#222222",
  gray_2: "#767676",
  green_1: "#3cb46e",
};

const device = {
  mobileS: `only screen and (max-width: ${deviceSizes.mobileS})`,
  mobileM: `only screen and (max-width: ${deviceSizes.mobileM})`,
  mobileL: `only screen and (max-width: ${deviceSizes.mobileL})`,
  tablet: `only screen and (max-width: ${deviceSizes.tablet})`,
  tabletL: `only screen and (max-width: ${deviceSizes.tabletL})`,
};

const theme = {
  fontSizes,
  colors,
  deviceSizes,
  device,
  paddings,
  margins,
  interval,
  verticalInterval,
  gradient,
};

export default theme;

```

저는 매 프로젝트에서 단위를 rem으로 사용하기 때문에 상단에 px을 rem으로 변경해 주는 함수를 하나 선언한 후, 자신이 자주 쓰는 단위나 프로젝트에서 지켜야 할 규격을 기준으로 Style 요소들을 지정합니다. 반응형에 대한 규격도 device 객체 안에 선언해 줬습니다.

# Component에서의 사용

Component에서 전달받은 theme를 사용해 보도록 하겠습니다.

- styled-component에서 사용하기

```
const NavbarWrapper = styled.div`
  display: flex;
  justify-content: space-between;
  align-items: center;

  // ${}를 사용해서 인자로 theme의 값을 받을 수 있고 원하는 theme의 요소를 사용할 수 있습니다.

  padding: ${({ theme }) => theme.paddings.xl};
  font-size: ${({ theme }) => theme.fontSizes.base};
`;

```

- Component 내부에서 사용하기

> Component 내부에서 내가 지정한 theme를 사용하는 경우도 있습니다. 간단한 예제로 저는 props로 원하는 Style을 전달해서 다양한 Button을 표현할 수 있는 Button Component를 만들었습니다. Button Component를 사용하려면 `<Button textColor="red" fillColor="green">`과 같이 props로 스타일 요소를 전달 해야 하고 저는 전달하는 스타일 요소를 제가 지정한 theme에서 선택해서 전달하고 싶습니다.

- Button

```
export const Button = styled.button`
  border-radius: 4px;
  height: 32px;
  font-size: 14px;
  line-height: 30px;
  padding: 0 ${({ theme }) => theme.paddings.lg};
  border: 1px solid transparent;

  color: ${(props) => (props.textColor ? props.textColor : "#000000")};
  background-color: ${(props) =>
    props.fillColor ? props.fillColor : "#FFFFFF"};

  :hover {
    transition: 0.3s;
    opacity: 0.8;
  }
`;

```

위에서 말한 것과 같이 ThemeProvider는 Context API와 같이 동작하며 useContext와 styled-component의 ThemeContext를 사용해서 theme의 값을 사용할 수 있습니다.

- 동작 예시

```
import React, { useContext } from "react";

import styled, { ThemeContext } from "styled-components";
import { Button } from "../../styles/Button";

const Navbar = () => {
  const themeContext = useContext(ThemeContext);

  return (
    <NavbarWrapper>
      <Logo></Logo>
      <Button
        fillColor={themeContext.colors.green_1}
        textColor={themeContext.colors.white}
      >
        Login
      </Button>
    </NavbarWrapper>
  );
};
```

# 후기

위 코드의 예시들은 현재 진행 중인 미니 프로젝트에 접목시키면서 공부한 내용을 담아 봤습니다.
처음 사용해 보는 기술이지만 확실히 전과 비교해서 불필요한 스타일 코드들도 사라지고 정해진 규격을 공유해서 조금이나마 일관된 스타일을 구성하는데 도움을 받았다고 생각됩니다.
