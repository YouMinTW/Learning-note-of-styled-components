# Ways play with styled-components

## 1.直接在 styled 預設提供的 HTML 元件，寫入 css 樣式

```tsx
const Button = styled.button`
  background: blue;
  color: white;
  &:hover {
    background: gray;
    color: black;
  }
`;

render(<Button> 點我！</Button>);
```

## 2.透過 props 動態改變 styled-components 的元件

```tsx
const Button = styled.button<{ primary?: boolean }>`
  background: ${props => (props.primary ? 'pink' : 'blue')};
  color: ${props => (props.primary ? 'white' : 'green')};
  &:hover {
    background: gray;
    color: black;
  }
`;

render(
  <div>
    <Button> 點我！</Button>
    <Button primary> 點我！</Button>
  </div>
);
```

## 3.基於某個 styled-components 之上，創造新的元件

```tsx
const Button = styled.button`
  background: blue;
  color: white;
  &:hover {
    background: gray;
    color: black;
  }
`;
const CoolButton = styled(Button)`
  background: yellow;
  color: pink;
  &:hover {
    background: pink;
    color: yellow;
  }
`;

render(
  <div>
    <Button> 點我！</Button>
    <CoolButton> 點我！</CoolButton>
  </div>
);
```

### 如此可知，我們便可以修改某些其他人寫好的 styled-components 了

> 如 MATERIAL-UI, Ant Design 等等，目前都有支援 styled-components 互動了

```tsx
import Button from '@material-ui/core/Button';
const ChillButton = styled(Button)`
  font-weight: 900;
  text-transform: inherit;
`;
render(
  <ChillButton variant="contained" color="primary">
    Yoyoyo 點我！
  </ChillButton>
);
```

### 題外話，與有支援的第三方套件互動？！(以 MATERIAL-UI 為例) 期待其他大大分享如 Ant Design 等相關經驗。

#### 有時候，我們會需要修改、覆寫到第三方套件預設的樣式，難不成要用到兇殘的 `!important`？

```tsx
import Button from '@material-ui/core/Button';
const ChillButton = styled(Button)`
  color: blue !important;
  font-weight: 900;
  text-transform: inherit;
`;
render(
  <ChillButton variant="contained" color="primary">
    Yoyoyo 點我！
  </ChillButton>
);
```

#### 也許你還有其他種解法，就是 MATERIAL-UI 提供改變注入樣式的優先順序的方法

```tsx
import { StylesProvider } from '@material-ui/core/styles';
render(
  <StylesProvider injectFirst>
    <App />
  </StylesProvider>
);
```

## 4.如何與其他無支援 styled-components 的元件互動及修改樣式呢？ （含其他第三方、自己的元件）

### 透過綁定 className 使 styled-components 能注入樣式，此範例來自[官網 styling-any-component](https://styled-components.com/docs/basics#styling-any-component)

```tsx
// from official document https://styled-components.com/docs/basics#styling-any-component
// This could be react-router-dom's Link for example
const Link = ({ className, children }) => (
  <a className={className}>{children}</a>
);

const StyledLink = styled(Link)`
  color: palevioletred;
  font-weight: bold;
`;

render(
  <div>
    <Link>Unstyled, boring Link</Link>
    <br />
    <StyledLink>Styled, exciting Link</StyledLink>
  </div>
);
```
