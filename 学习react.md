### 快速入门

#### 创建和嵌套组件

React应用程序是由组件组成的。一个组件是UI（用户界面）的一部分，它拥有自己的逻辑和外观。组件可以小到一个按钮，也可以大到整个页面。

react组件是返回标签的JavaScript函数：

```javascript
function MyButton(){
    return (
    <button>我是一个按钮<button>
)
}
```

至此，你已经声明了MyButton，现在把它嵌套到另一个组件中：

```javascript
export default function MyApp(){
    return (
    <div>
    <h1>欢迎来到我的APP</h1>
<MyButton/>
<div/>
)
}
```

你可能已经注意到<MyButton/>是以大写字母开头的。你可以据此识别React组件。React组件必须以大写字母开头，而HTML标签则必须是小写字母。

#### 使用JSX编写标签

上面所使用的标签语法被称为JSX。它是可选的，但大多数React项目会使用JSX，主要是他很方便。

JX比HTML更加严格。你必须闭合标签，如<br /> 你必须将它们包裹到一个共享的父级中，比如 `<div>...</div>` 或使用空的 `<>...</>` 包裹：

```javascript
function AboutPage() {
  return (
    <>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
    </>
  );
}
```

#### 添加样式

在 React 中，你可以使用 `className` 来指定一个 CSS 的 class。它与 HTML 的 [`class`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/class) 属性的工作方式相同：

```jsx
<img className="avatar" />
```

然后，你可以在一个单独的 CSS 文件中为它编写 CSS 规则：

```css
/* In your CSS */
.avatar {
  border-radius: 50%;
}
```

React 并没有规定你如何添加 CSS 文件。最简单的方式是使用 HTML 的 [`<link>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link) 标签。如果你使用了构建工具或框架，请阅读其文档来了解如何将 CSS 文件添加到你的项目中。

#### 显示数据

JSX 会让你把标签放到 JavaScript 中。而大括号会让你 “回到” JavaScript 中，这样你就可以从你的代码中嵌入一些变量并展示给用户。例如，这将显示 `user.name`：

```jsx
return (
  <h1>
    {user.name}
  </h1>
);
```

你还可以将 JSX 属性 “转义到 JavaScript”，但你必须使用大括号 **而非** 引号。例如，`className="avatar"` 是将 `"avatar"` 字符串传递给 `className`，作为 CSS 的 class。但 `src={user.imageUrl}` 会读取 JavaScript 的 `user.imageUrl` 变量，然后将该值作为 `src` 属性传递：

```jsx
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

你也可以把更为复杂的表达式放入 JSX 的大括号内，例如 [字符串拼接](https://javascript.info/operators#string-concatenation-with-binary)：

```js
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

在上面示例中，`style={{}}` 并不是一个特殊的语法，而是 `style={ }` JSX 大括号内的一个普通 `{}` 对象。当你的样式依赖于 JavaScript 变量时，你可以使用 `style` 属性。

#### 条件渲染

React 没有特殊的语法来编写条件语句，因此你使用的就是普通的 JavaScript 代码。例如使用 [`if`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/if...else) 语句根据条件引入 JSX：

```jsx
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

如果你喜欢更为紧凑的代码，可以使用 [条件 `?` 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。与 `if` 不同的是，它工作于 JSX 内部：

```jsx
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

当你不需要 `else` 分支时，你还可以使用 [逻辑 `&&` 语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation)：

```jsx
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

所有这些方法也适用于有条件地指定属性。如果你对 JavaScript 语法不熟悉，你可以从一直使用 `if...else` 开始。

#### 渲染列表

你将依赖 JavaScript 的特性，例如 [`for` 循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for) 和 [array 的 `map()` 函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 来渲染组件列表。

假设你有一个产品数组：

```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
```

在你的组件中，使用 `map()` 函数将这个数组转换为 `<li>` 标签构成的列表:

```jsx
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

注意， `<li>` 有一个 `key` 属性。对于列表中的每一个元素，你都应该传递一个字符串或者数字给 `key`，用于在其兄弟节点中唯一标识该元素。通常 key 来自你的数据，比如数据库中的 ID。如果你在后续插入、删除或重新排序这些项目，React 将依靠你提供的 key 来思考发生了什么。

```js
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

#### 响应事件

你可以通过在组件中声明 **事件处理** 函数来响应事件：

```jsx
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

注意，`onClick={handleClick}` 的结尾没有小括号！不要 **调用** 事件处理函数：你只需 **把函数传递给事件** 即可。当用户点击按钮时 React 会调用你传递的事件处理函数。

#### 更新界面

通常你会希望你的组件 “记住” 一些信息并展示出来，比如一个按钮被点击的次数。要做到这一点，你需要在你的组件中添加 **state**。

首先，从 React 引入 [`useState`](https://react.docschina.org/reference/react/useState)：

```jsx
import { useState } from 'react';
```

现在你可以在你的组件中声明一个 **state 变量**：

```jsx
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

你将从 `useState` 中获得两样东西：当前的 state（`count`），以及用于更新它的函数（`setCount`）。你可以给它们起任何名字，但按照惯例会像 `[something, setSomething]` 这样为它们命名。

第一次显示按钮时，`count` 的值为 `0`，因为你把 `0` 传给了 `useState()`。当你想改变 state 时，调用 `setCount()` 并将新的值传递给它。点击该按钮计数器将递增：

```jsx
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

React 将再次调用你的组件函数。第一次 `count` 变成 `1`。接着点击会变成 `2`。继续点击会逐步递增。

如果你多次渲染同一个组件，每个组件都会拥有自己的 state。你可以尝试点击不同的按钮：

```jsx
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

注意，每个按钮会 “记住” 自己的 `count`，而不影响其他按钮。

#### 使用Hook

以 `use` 开头的函数被称为 **Hook**。`useState` 是 React 提供的一个内置 Hook。你可以在 [React API 参考](https://react.docschina.org/reference/react) 中找到其他内置的 Hook。你也可以通过组合现有的 Hook 来编写属于你自己的 Hook。

Hook 比普通函数更为严格。你只能在你的组件（或其他 Hook）的 **顶层** 调用 Hook。如果你想在一个条件或循环中使用 `useState`，请提取一个新的组件并在组件内部使用它。

#### 组件间共享数据

在前面的示例中，每个 `MyButton` 都有自己独立的 `count`，当每个按钮被点击时，只有被点击按钮的 `count` 才会发生改变：

![](C:\Users\18655\AppData\Roaming\marktext\images\2024-04-25-16-44-05-image.png)

然而，你经常需要组件 **共享数据并一起更新**。

为了使得 `MyButton` 组件显示相同的 `count` 并一起更新，你需要将各个按钮的 state “向上” 移动到最接近包含所有按钮的组件之中。

在这个示例中，它是 `MyApp`：

![](C:\Users\18655\AppData\Roaming\marktext\images\2024-04-25-16-44-44-image.png)

此刻，当你点击任何一个按钮时，`MyApp` 中的 `count` 都将改变，同时会改变 `MyButton` 中的两个 count。具体代码如下：

首先，将 `MyButton` 的 **state 上移到** `MyApp` 中：

```jsx
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... we're moving code from here ...
}
```

接着，将 `MyApp` 中的点击事件处理函数以及 **state 一同向下传递到** 每个 `MyButton` 中。你可以使用 JSX 的大括号向 `MyButton` 传递信息。就像之前向 `<img>` 等内置标签所做的那样:

```jsx
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```

使用这种方式传递的信息被称作 **prop**。此时 `MyApp` 组件包含了 `count` state 以及 `handleClick` 事件处理函数，并将它们作为 **prop 传递给** 了每个按钮。

最后，改变 `MyButton` 以 **读取** 从父组件传递来的 prop：

```jsx
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

当你点击按钮时，`onClick` 处理程序会启动。每个按钮的 `onClick` prop 会被设置为 `MyApp` 内的 `handleClick` 函数，所以函数内的代码会被执行。该代码会调用 `setCount(count + 1)`，使得 state 变量 `count` 递增。新的 `count` 值会被作为 prop 传递给每个按钮，因此它们每次展示的都是最新的值。这被称为“状态提升”。通过向上移动 state，我们实现了在组件间共享它。

```jsx
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

### React哲学

React 可以改变你对可见设计和应用构建的思考。当你使用 React 构建用户界面时，你首先会把它分解成一个个 **组件**，然后，你需要把这些组件连接在一起，使数据流经它们。在本教程中，我们将引导你使用 React 构建一个可搜索的产品数据表。

#### 从原型开始

想象一下，你早已从设计者那儿得到了一个 JSON API 和原型。

JSON API 返回如下的数据:

```jsx
[
  { category: "Fruits", price: "$1", stocked: true, name: "Apple" },
  { category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit" },
  { category: "Fruits", price: "$2", stocked: false, name: "Passionfruit" },
  { category: "Vegetables", price: "$2", stocked: true, name: "Spinach" },
  { category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin" },
  { category: "Vegetables", price: "$1", stocked: true, name: "Peas" }
]
```

原型看起来像是这样:

<img src="https://react.docschina.org/images/docs/s_thinking-in-react_ui.png" />

仅需跟随下面的五步，即可使用 React 来实现 UI。

#### 步骤一：将UI拆解为组件层级结构

一开始，在绘制原型中的每个组件和子组件周围绘制盒子并命名它们。如果你与设计师一起工作，他们可能早已在其设计工具中对这些组件进行了命名。检查一下它们!

取决于你的使用背景，可以考虑通过不同的方式将设计分割为组件:

- **程序设计**——使用同样的技术决定你是否应该创建一个新的函数或者对象。这一技术即 [单一功能原理](https://en.wikipedia.org/wiki/Single_responsibility_principle)，也就是说，一个组件理想情况下应仅做一件事情。但随着功能的持续增长，它应该被分解为更小的子组件。
- **CSS**——思考你将把类选择器用于何处。(然而，组件并没有那么细的粒度。)
- **设计**——思考你将如何组织布局的层级。

如果你的 JSON 结构非常棒，经常会发现其映射到 UI 中的组件结构是一件自然而然的事情。那是因为 UI 和原型常拥有相同的信息结构—即，相同的形状。将你的 UI 分割到组件，每个组件匹配到原型中的每个部分。

以下展示了五个组件:

![](https://react.docschina.org/images/docs/s_thinking-in-react_ui_outline.png)

1. `FilterableProductTable`（灰色）包含完整的应用。

2. `SearchBar`（蓝色）获取用户输入。

3. `ProductTable`（淡紫色）根据用户输入，展示和过滤清单。

4. `ProductCategoryRow`（绿色）展示每个类别的表头。

5. `ProductRow`（黄色）展示每个产品的行。

看向 `ProductTable`（淡紫色），可以看到表头（包含 “Name” 和 “Price” 标签）并不是独立的组件。这是个人喜好的问题，你可以采取任何一种方式继续。在这个例子中，它是作为 `ProductTable` 的一部分，因为它展现在 `ProductTable` 列表之中。然而，如果这个表头变得复杂（举个例子，如果添加排序），创建独立的 `ProductTableHeader` 组件就变得有意义了。

现在你已经在原型中辨别了组件，并将它们转化为了层级结构。在原型中，组件可以展示在其它组件之中，在层级结构中如同其孩子一般:

- `FilterableProductTable`
  - `SearchBar`
  - `ProductTable`
    - `ProductCategoryRow`
    - `ProductRow`

#### 步骤二：使用React构建一个静态版本

现在你已经拥有了你自己的组件层级结构，是时候实现你的应用程序了。最直接的办法是根据你的数据模型，构建一个不带任何交互的 UI 渲染代码版本…经常是先构建一个静态版本比较简单，然后再一个个添加交互。构建一个静态版本需要写大量的代码，并不需要什么思考; 但添加交互需要大量的思考，却不需要大量的代码。

构建应用程序的静态版本来渲染你的数据模型，将构建 [组件](https://react.docschina.org/learn/your-first-component) 并复用其它的组件，然后使用 [props](https://react.docschina.org/learn/passing-props-to-a-component) 进行传递数据。Props 是从父组件向子组件传递数据的一种方式。如果你对 [state](https://react.docschina.org/learn/state-a-components-memory) 章节很熟悉，不要在静态版本中使用 state 进行构建。state 只是为交互提供的保留功能，即数据会随着时间变化。因为这是一个静态应用程序，所以并不需要。

你既可以通过从层次结构更高层组件（如 `FilterableProductTable`）开始“自上而下”构建，也可以通过从更低层级组件（如 `ProductRow`）“自下而上”进行构建。在简单的例子中，自上而下构建通常更简单；而在大型项目中，自下而上构建更简单。

```jsx
function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar() {
  return (
    <form>
      <input type="text" placeholder="Search..." />
      <label>
        <input type="checkbox" />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

function FilterableProductTable({ products }) {
  return (
    <div>
      <SearchBar />
      <ProductTable products={products} />
    </div>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

如果你无法理解这段代码，请先阅读 [快速入门](https://react.docschina.org/learn) 章节！

在构建你的组件之后，即拥有一个渲染数据模型的可复用组件库。因为这是一个静态应用程序，组件仅返回 JSX。最顶层组件（`FilterableProductTable`）将接收你的数据模型作为其 prop。这被称之为 **单向数据流**，因为数据从树的顶层组件传递到下面的组件。

#### 步骤三：找出 UI 精简且完整的 state 表示

为了使 UI 可交互，你需要用户更改潜在的数据结构。你将可以使用 **state** 进行实现。

考虑将 state 作为应用程序需要记住改变数据的最小集合。组织 state 最重要的一条原则是保持它 [DRY（不要自我重复）](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)。计算出你应用程序需要的绝对精简 state 表示，按需计算其它一切。举个例子，如果你正在构建一个购物列表，你可将他们在 state 中存储为数组。如果你同时想展示列表中物品数量，不需要将其另存为一个新的 state。取而代之，可以通过读取你数组的长度来实现。

现在考虑示例应用程序中的每一条数据:

1. 产品原始列表
2. 搜索用户键入的文本
3. 复选框的值
4. 过滤后的产品列表

其中哪些是 state 呢？标记出那些不是的:

- 随着时间推移 **保持不变**？如此，便不是 state。
- 通过 props **从父组件传递**？如此，便不是 state。
- 是否可以基于已存在于组件中的 state 或者 props **进行计算**？如此，它肯定不是state！

剩下的可能是 state。

让我们再次一条条验证它们:

1. 原始列表中的产品 **被作为 props 传递，所以不是 state**。
2. 搜索文本似乎应该是 state，因为它会随着时间的推移而变化，并且无法从任何东西中计算出来。
3. 复选框的值似乎是 state，因为它会随着时间的推移而变化，并且无法从任何东西中计算出来。
4. 过滤后列表中的产品 **不是 state，因为可以通过被原始列表中的产品，根据搜索框文本和复选框的值进行计算**。

这就意味着只有搜索文本和复选框的值是 state！非常好！

在 React 中有两种“模型”数据：props 和 state。下面是它们的不同之处:

- [**props** 像是你传递的参数](https://react.docschina.org/learn/passing-props-to-a-component) 至函数。它们使父组件可以传递数据给子组件，定制它们的展示。举个例子，`Form` 可以传递 `color` prop 至 `Button`。
- [**state** 像是组件的内存](https://react.docschina.org/learn/state-a-components-memory)。它使组件可以对一些信息保持追踪，并根据交互来改变。举个例子，`Button` 可以保持对 `isHovered` state 的追踪。

props 和 state 是不同的，但它们可以共同工作。父组件将经常在 state 中放置一些信息（以便它可以改变），并且作为子组件的属性 **向下** 传递至它的子组件。如果第一次了解这其中的差别感到迷惑，也没关系。通过大量练习即可牢牢记住！

#### 步骤四：验证 state 应该被放置在哪里

在验证你应用程序中的最小 state 数据之后，你需要验证哪个组件是通过改变 state 实现可响应的，或者 **拥有** 这个 state。记住：React 使用单向数据流，通过组件层级结构从父组件传递数据至子组件。要搞清楚哪个组件拥有哪个 state。如果你是第一次阅读此章节，可能会很有挑战，但可以通过下面的步骤搞定它!

为你应用程序中的每一个 state:

1. 验证每一个基于特定 state 渲染的组件。
2. 寻找它们最近并且共同的父组件——在层级结构中，一个凌驾于它们所有组件之上的组件。
3. 决定 state 应该被放置于哪里:
   1. 通常情况下，你可以直接放置 state 于它们共同的父组件。
   2. 你也可以将 state 放置于它们父组件上层的组件。
   3. 如果你找不到一个有意义拥有这个 state 的地方，单独创建一个新的组件去管理这个 state，并将它添加到它们父组件上层的某个地方。

在之前的步骤中，你已在应用程序中创建了两个 state：输入框文本和复选框的值。在这个例子中，它们总在一起展示，将其视为一个 state 非常简单。

现在为这个 state 贯彻我们的策略:

1. **验证使用 state 的组件**：
   - `ProductTable` 需要基于 state (搜索文本和复选框值) 过滤产品列表。
   - `SearchBar` 需要展示 state (搜索文本和复选框值)。
2. **寻找它们的父组件**：它们的第一个共同父组件为 `FilterableProductTable`。
3. **决定 state 放置的地方**：我们将放置过滤文本和勾选 state 的值于 `FilterableProductTable`。

所以 state 将被放置在 `FilterableProductTable`。

用 [`useState()` Hook](https://react.docschina.org/reference/react/useState) 为组件添加 state。Hook 可以“钩住”组件的 [渲染周期](https://react.docschina.org/learn/render-and-commit)。在 `FilterableProductTable` 的顶部添加两个 state 变量，用于指定你应用程序的初始 state：

```jsx
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);
```

然后，`filterText` 和 `inStockOnly` 作为 props 传递至 `ProductTable` 和 `SearchBar`：

```jsx
<div>
  <SearchBar
    filterText={filterText}
    inStockOnly={inStockOnly} />
  <ProductTable
    products={products}
    filterText={filterText}
    inStockOnly={inStockOnly} />
</div>
```

你可以查看你应用程序的表现。在下面的沙盒代码中，通过修改 `useState('')` 为 `useState('fruit')` 以编辑 `filterText` 的初始值，你将会发现搜索输入框和表格发生更新：

#### 步骤五：添加反向数据流

目前你的应用程序可以带着 props 和 state 随着层级结构进行正确渲染。但是根据用户的输入改变 state，需要通过其它的方式支持数据流：深层结构的表单组件需要在 `FilterableProductTable` 中更新 state。

React 使数据流显式展示，是与双向数据绑定相比，需要更多的输入。如果你尝试在上述的例子中输入或者勾选复选框，发现 React 忽视了你的输入。这点是有意为之的。通过 `<input value={filterText} />`，已经设置了 `input` 的 `value` 属性，使之恒等于从 `FilterableProductTable` 传递的 `filterText` state。只要 `filterText` state 不设置，（输入框的）输入就不会改变。

当用户更改表单输入时，state 将更新以反映这些更改。state 由 `FilterableProductTable` 所拥有，所以只有它可以调用 `setFilterText` 和 `setInStockOnly`。使 `SearchBar` 更新 `FilterableProductTable` 的 state，需要将这些函数传递到 `SearchBar`：

在 `SearchBar` 中，添加一个 `onChange` 事件处理器，使用其设置父组件的 state：

现在应用程序可以完整工作了！

```jsx
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar
        filterText={filterText}
        inStockOnly={inStockOnly}
        onFilterTextChange={setFilterText}
        onInStockOnlyChange={setInStockOnly} />
      <ProductTable
        products={products}
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input
        type="text"
        value={filterText} placeholder="Search..."
        onChange={(e) => onFilterTextChange(e.target.value)} />
      <label>
        <input
          type="checkbox"
          checked={inStockOnly}
          onChange={(e) => onInStockOnlyChange(e.target.checked)} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

### 安装

React 从诞生之初就是可被渐进式使用的。因此你可以选择性地使用 React 特性。不管你是想体验下 React，用它为简单的 HTML 页面增加交互，还是重新搭建一个由 React 驱动的复杂应用，本章节内容都能帮你快速入门。

#### 尝试 React

无需进行任何安装，即可体验：

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}</h1>;
}

export default function App() {
  return <Greeting name="world" />
}
```

#### 本地尝试React

如果想在电脑本地上进行尝试, [下载这个HTML页面](https://gist.githubusercontent.com/gaearon/0275b1e1518599bbeafcde4722e79ed1/raw/db72dcbf3384ee1708c4a07d3be79860db04bff0/example.html)。 然后在你的编辑器和浏览器中打开!

#### 开始一个新的React项目

如果你想完全使用 React 建立一个应用或者一个网站, [开始一个新的 React 项目](https://react.docschina.org/learn/start-a-new-react-project)。

#### 添加React到一个已有的项目

如果你想在一个现有的应用或者网站上尝试 React, [添加 React 到一个现有的项目](https://react.docschina.org/learn/add-react-to-an-existing-project)。

### 启动一个新的React 项目

如果你想完全用 React 构建一个新的应用或网站，我们建议选择社区中流行的、由 React 驱动的框架。

你可以在没有框架的情况下使用 React，但是我们发现大多数应用程序和网站最终都会构建常见问题的解决方案，例如代码分割、路由、数据获取和生成 HTML。不仅仅是 React，这些问题对于所有 UI 库都很常见。

从框架开始一个项目，你就可以快速使用 React，这样以后也不需要构建自己的框架。

> 你当然可以在没有框架的情况下使用 React——这也就是你将 [使用 React 作为页面的一部分](https://react.docschina.org/learn/add-react-to-an-existing-project#using-react-for-a-part-of-your-existing-page)。**但是，如果你完全使用 React 构建新应用程序或网站，我们建议使用框架。**
> 
> 原因如下。
> 
> 即使你一开始不需要路由或数据获取，你也可能需要为它们添加一些库。随着 JavaScript 包随着每个新功能的增加而增长，你可能必须弄清楚如何单独拆分每个路由的代码。随着数据获取需求变得更加复杂，你可能会遇到服务器-客户端网络瀑布，这会让你的应用程序让人感觉非常缓慢。由于你的受众里面有更多网络条件较差和低端设备的用户，你可能需要从组件生成 HTML 以便尽早显示内容（无论是在服务器上还是在构建期间）。更改设定以在服务器上或构建期间运行某些代码可能非常棘手。
> 
> **这些问题不是 React 特有的。这就是为什么 Svelte 有 SvelteKit、Vue 有 Nuxt 等等。要自己解决这些问题，需要将打包工具与路由和数据获取的库集成**。让初始设定正常工作并不难，但是要制作一个即使随着时间的推移而增长也能快速加载的应用程序，涉及到很多微妙之处。你需要发送最少量的应用程序代码，在单个客户端-服务器往返中执行此操作，与页面所需的任何数据并行。你可能希望页面在 JavaScript 代码运行之前就具有交互性，以支持渐进增强。你可能希望为你营销页面生成一个包含纯静态 HTML 文件的文件夹，该文件夹可以托管在任何地方，并且在禁用 JavaScript 的情况下仍然可以工作。自己构建这些东西需要付出实际的努力。
> 
> 此页面上的 React 框架默认解决此类问题，无需进行额外的工作。它们让你能够非常精简地开始，然后根据你的需求扩展应用程序。每个 React 框架都有一个社区，因此寻找问题的答案和升级工具变得更加容易。框架还为您的代码提供结构，帮助你和其他人保留不同项目之间的上下文和技巧。相反，使用自定义设置更容易陷入不受支持的依赖版本，并且你基本上最终会创建自己的框架——但这没有社区支持或升级路径（如果它和我们过去做的一样，设计得比较草率的话）。
> 
> 如果你的应用程序具有这些框架无法很好地满足的异常约束，或者你更喜欢自己解决这些问题，则可以使用 React 进行自己的自定义设置。从 npm 获取 `react` 和 `react-dom`，使用 [Vite](https://cn.vitejs.dev/) 或 [Parcel](https://parceljs.org/) 等打包工具设置自定义构建流程，并根据需要添加其他工具用于路由、静态内容生成或服务端渲染等。

#### 生产级的React框架

这些框架支持在生产中部署和扩展应用程序所需的所有功能，并致力于支持我们的 [全栈架构愿景](https://react.docschina.org/learn/start-a-new-react-project#which-features-make-up-the-react-teams-full-stack-architecture-vision)。我们推荐的所有框架都是开源的，有活跃的社区支持，并且可以部署到你自己的服务器或托管服务提供商。如果你是一位框架作者，有兴趣加入此列表，[请告诉我们](https://github.com/reactjs/react.dev/issues/new?assignees=&labels=type%3A+framework&projects=&template=3-framework.yml&title=%5BFramework%5D%3A+)。

#### Next.js

**[Next.js 的页面路由](https://nextjs.org/) 是一个全栈的 React 框架**。它用途广泛，可让你创建任何规模的 React 应用程序——从大部分的静态博客到复杂的动态应用程序。要创建新的 Next.js 项目，请在终端中运行：

```js
npx create-next-app@latest
```

如果你是 Next.js 的新手，请查看 [Next.js 课程](https://nextjs.org/learn)。

Next.js 由 [Vercel](https://vercel.com/) 维护。你可以 [将 Next.js 应用](https://nextjs.org/docs/app/building-your-application/deploying) 部署到 Node.js 或 serverless 上，也可以部署到你自己的服务器上。[完全静态的 Next.js 应用](https://nextjs.org/docs/pages/building-your-application/deploying/static-exports) 可以部署在任何支持静态服务的地方。

#### Remix

**[Remix](https://remix.run/) 是一个具有嵌套路由的全栈式 React 框架**。它可以把你的应用分成嵌套部分，该嵌套部分可以并行加载数据并响应用户操作进行刷新。要创建一个新的 Remix 项目，请运行：

```js
npx create-remix
```

如果你是 Remix 的新手，请查看 Remix 的 [博客教程](https://remix.run/docs/en/main/tutorials/blog)（短）和 [应用教程](https://remix.run/docs/en/main/tutorials/jokes)（长）。

Remix 由 [Shopify](https://www.shopify.com/) 维护。当你创建一个 Remix 项目时，你需要 [选择你的部署目标](https://remix.run/docs/en/main/guides/deployment)。你可以通过使用或编写 [适配器](https://remix.run/docs/en/main/other-api/adapter) 将 Remix 应用部署到 Node.js 或 serverless 上进行托管。

#### Gatsby

**[Gatsby](https://www.gatsbyjs.com/) 是一个快速的支持 CMS 的网站的 React 框架**。其丰富的插件生态系统和 GraphQL 数据层简化了将内容、API 和服务整合到一个网站的过程。要创建一个新的 Gatsby 项目，请运行：

```js
npx create-gatsby
```

如果你是 Gatsby 的新手，请查看 [Gatsby 教程](https://www.gatsbyjs.com/docs/tutorial/)。

Gatsby 由 [Netlify](https://www.netlify.com/) 维护。你可以 [部署一个完全静态的 Gatsby 网站](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting) 到任何一个支持静态服务的地方。如果你选择使用服务器上的功能，请确保你的主机供应商支持 Gatsby 的功能。

#### Expo（用于原生应用）

**[Expo](https://expo.dev/) 是一个 React 框架，可以让你创建具有真正原生 UI 的应用，包括 Android、iOS，以及 Web 应用**。它为 [React Native](https://reactnative.dev/) 提供了 SDK，使原生部分更容易使用。要创建一个新的 Expo 项目，请运行：

```js
npx create-expo-app
```

如果你是 Expo 的新手，请查看 [Expo 教程](https://docs.expo.dev/tutorial/introduction/)。

Expo 是由 [Expo 这家公司](https://expo.dev/about) 维护的。用 Expo 构建应用是免费的，而且你可以不受限制地将它们提交到谷歌和苹果的应用商店。此外，Expo 还提供选择性的付费云服务。

#### Bleeding-edge React frameworks

在我们探索如何继续改进 React 的过程中，我们意识到将 React 与框架（特别是路由、bundle 和服务端技术）更紧密地结合起来是我们帮助 React 用户构建更好的应用的最大机会。Next.js 团队已经同意与我们合作，研究、开发、集成和测试与框架无关的 React 前沿功能，如 [React 服务器组件](https://react.docschina.org/blog/2023/03/22/react-labs-what-we-have-been-working-march-2023#react-server-components)。

这些功能每天都在接近生产就绪，而且我们一直在与其他 bundler 和框架的开发者讨论整合它们。我们希望在一两年内，这个页面上列出的所有框架都能完全支持这些功能。（如果你是一个框架的作者，有兴趣与我们合作来实验这些功能，请告诉我们！）

#### Next.js (App Router)

**[Next.js 的 App Router](https://nextjs.org/docs) 是对 Next.js API 的重新设计，旨在实现 React 团队的全栈架构愿景**。它让你在异步组件中获取数据，这些组件甚至能在服务端构建过程中运行。

Next.js 由 [Vercel](https://vercel.com/) 维护。你可以将 [Next.js 应用](https://nextjs.org/docs/app/building-your-application/deploying) 部署到 Node.js 或 serverless 主机上，或部署到你自己的服务器上。Next.js 还支持 [静态导出](https://nextjs.org/docs/app/building-your-application/deploying/static-exports)，不需要服务器。

> #### 哪些功能构成了 React 团队的全栈架构愿景

> Next.js 的 App Router bundler 完全实现了官方的 [React 服务器组件规范](https://github.com/reactjs/rfcs/blob/main/text/0188-server-components.md)。这让你可以在一棵 React 树上同时使用 *构建时*、*纯服务端* 和 *交互组件*。
> 
> 例如，你可以把一个纯服务端的 React 组件写成一个 `async` 函数，从数据库或文件中读取。然后你可以把数据从它那里传递给你的交互组件：
> 
> ```
> // 这个组件只在服务端运行（或在构建期间）。async function Talks({ confId }) {  // 1. 你在服务端，所以你可以和你的数据层对话。不需要 API 端点。  const talks = await db.Talks.findAll({ confId });  // 2. 添加任意数量的渲染逻辑。它不会使你的 JavaScript bundle 变大。  const videos = talks.map(talk => talk.video);  // 3. 将数据向下传递给将在浏览器中运行的组件。  return <SearchableVideoList videos={videos} />;}
> ```
> 
> Next.js 的 App Router 还集成了 [用 Suspense 获取数据的能力](https://react.docschina.org/blog/2022/03/29/react-v18#suspense-in-data-frameworks)。这让你可以直接在 React 树中为用户界面的不同部分指定一个加载状态（像一个骨架占位符）：
> 
> ```
> <Suspense fallback={<TalksLoading />}>  <Talks confId={conf.id} /></Suspense>
> ```
> 
> 服务器组件和 Suspense 是 React 的功能，不是由 Next.js 提供的。然而，在框架层面上采用它们需要投入大量的实现工作。目前，Next.js App Router 是最完整的实现。React 团队正在与 bundler 的开发人员合作，使这些功能在下一代框架中更容易实现。

### 将React添加到现有项目中

如果想对现有项目添加一些交互，不必使用 React 将其整个重写。只需将 React 添加到已有技术栈中，就可以在任何位置渲染交互式的 React 组件。

#### 在现有网站的子路由中使用React

假设你在 `example.com` 部署了一个其他服务端技术（例如 Rails）构建的 Web 应用，但是你又想在 `example.com/some-app/` 部署一个 React 项目。

以下是推荐的配置方式：

1. 使用一个 [基于 React 的框架](https://react.docschina.org/learn/start-a-new-react-project) 构建 **应用的 React 部分**。
2. **在框架配置中将 `/some-app` 指定为基本路径**（这里有 [Next.js](https://nextjs.org/docs/api-reference/next.config.js/basepath) 与 [Gatsby](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting/path-prefix/) 的配置样例）。
3. **配置服务器或代理**，以便所有位于 `/some-app/` 下的请求都由 React 应用处理。

这可以确保应用的 React 部分可以受益于这些框架中内置的 [最佳实践](https://react.docschina.org/learn/start-a-new-react-project#can-i-use-react-without-a-framework)。

许多基于 React 的框架都是全栈的，从而可以让你的 React 应用充分利用服务器。但是，即使无法或不想在服务器上运行 JavaScript，也可以使用相同的方法。在这种情况下，将 HTML/CSS/JS 导出（Next.js 的 [`next export` output](https://nextjs.org/docs/advanced-features/static-html-export)，Gatsby 的 default）替换为 `/some-app/`。

#### 在现有页面的一部分中使用React

假设有一个其他技术栈（无论是 Rails 这样的服务端技术，还是 Backbone 那样的客户端技术）构建的现有页面，并且想要在该页面的某个位置渲染交互式的 React 组件。这是集成 React 的常见方式——实际上，这也正是多年来大多数情况下 Meta 使用 React 的方式！

你可以分两步进行：

1. **配置 JavaScript 环境**，以便使用 [JSX 语法](https://react.docschina.org/learn/writing-markup-with-jsx)、[`import`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import) 和 [`export`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/export) 语法将代码拆分为模块，以及从 [npm](https://www.npmjs.com/) 包注册表中使用包（例如 React）。
2. **在需要的位置渲染 React 组件**。

确切的方法取决于现有的页面配置，因此让我们对一些细节进行说明。

#### 步骤1：配置模块化的JavaScript 环境

模块化的 JavaScript 环境可以让你在单一的文件中编写 React 组件，而不是在一个文件中编写所有的代码。它还可以让你使用其他开发人员在 [npm](https://www.npmjs.com/) 注册表上发布的一些特别好用的包，包括 React！如何实现这一点取决于你现有的配置：

- **如果你的应用已经使用 `import` 语句来分割成不同的文件，请尝试利用已有的配置**。检查在你的 JavaScript 代码中编写 `<div />`是否会导致语法错误。如果有语法错误，你可能需要使用 [Babel](https://babeljs.io/setup) 转换你的 JavaScript 代码，并启用 [Babel React preset](https://babeljs.io/docs/babel-preset-react) 来使用 JSX。

- **如果你的应用没有用于编译 JavaScript 模块的配置，请使用 [Vite](https://cn.vitejs.dev/) 进行配置**。Vite 社区维护了与后端框架（包括 Rails、Django 和 Laravel）的 [许多集成项目](https://github.com/vitejs/awesome-vite#integrations-with-backends)。如果你的后端框架没有列出，请 [按照此指南](https://cn.vitejs.dev/guide/backend-integration.html) 手动将 Vite 构建集成到你的后端。

如果想要检查你的配置是否有效，可以在项目文件夹中运行以下命令：

```jsx
npm install react react-dom
```

然后在你的 JavaScript 主文件（它可能被称为 `index.js` 或 `main.js`）的顶部添加以下代码：

```jsx
import { createRoot } from 'react-dom/client';

// 清除现有的 HTML 内容
document.body.innerHTML = '<div id="app"></div>';

// 渲染你的 React 组件
const root = createRoot(document.getElementById('app'));
root.render(<h1>Hello, world</h1>);
```

第一次将模块化 JavaScript 环境集成到现有项目中可能会让人感到害怕，但这是值得的！如果遇到困难，请尝试我们的 [社区资源](https://react.docschina.org/community) 或 [Vite Chat](https://chat.vitejs.dev/)。

#### 步骤2：在页面的任何位置渲染React组件

在上一步中，此代码将被放在主文件的顶部：

```jsx
import { createRoot } from 'react-dom/client';

// 清除现有的 HTML 内容
document.body.innerHTML = '<div id="app"></div>';

// 渲染你的 React 组件
const root = createRoot(document.getElementById('app'));
root.render(<h1>Hello, world</h1>);
```

当然，你实际上并不想清除现有的 HTML 内容！

那么请删除此代码。

相反，你可能想要在 HTML 中特定的位置渲染 React 组件。打开 HTML 页面（或用于生成它的服务端模板），并向任意一个标签添加一个唯一的 [`id`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/id) 属性，例如：

```jsx
<!-- ... 你的 HTML 代码某处 ... -->
<nav id="navigation"></nav>
<!-- ... 其他 HTML 代码 ... -->
```

这样可以使用 [`document.getElementById`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById) 查找到该 HTML 元素，并将其传递给 [`createRoot`](https://react.docschina.org/reference/react-dom/client/createRoot)，以便可以在其中渲染自己的 React 组件：

```jsx
import { createRoot } from 'react-dom/client';

function NavigationBar() {
  // TODO: 实际实现一个导航栏
  return <h1>Hello from React!</h1>;
}

const domNode = document.getElementById('navigation');
const root = createRoot(domNode);
root.render(<NavigationBar />);
```

请注意 `index.html` 中的原始 HTML 内容是如何保留的，但现在你自己的 `NavigationBar` React 组件出现在 HTML 的 `<nav id="navigation">` 中。阅读 [`createRoot` 用法文档](https://react.docschina.org/reference/react-dom/client/createRoot#rendering-a-page-partially-built-with-react) 以了解如何在现有 HTML 页面中渲染 React 组件。

当在现有项目中采用 React 时，通常会从小型交互式组件（例如按钮）开始，然后逐渐“向上移动”，直到最终整个页面都由 React 构建。到那个时候，我们建议立即迁移到 [一个 React 框架](https://react.docschina.org/learn/start-a-new-react-project)，以充分利用 React 的优势。

#### 在现有的原生移动应用中使用React Native

[React Native](https://reactnative.dev/) 也可以逐步集成到现有的原生应用中。如果已经有一个现有的 Android（Java 或 Kotlin）或 iOS（Objective-C 或 Swift）原生应用，请 [按照本指南](https://reactnative.dev/docs/integration-with-existing-apps) 将 React Native 添加到其中。

### 使用TypeScript

TypeScript 是一种向 JavaScript 代码添加类型定义的常用方法。TypeScript 天然支持 JSX——只需在项目中添加 [`@types/react`](https://www.npmjs.com/package/@types/react) 和 [`@types/react-dom`](https://www.npmjs.com/package/@types/react-dom) 即可获得完整的 React Web 支持。

#### 安装

所有的 [生产级 React 框架](https://react.docschina.org/learn/start-a-new-react-project#production-grade-react-framework) 都支持使用 TypeScript。请按照框架特定的指南进行安装：

- [Next.js](https://nextjs.org/docs/pages/building-your-application/configuring/typescript)
- [Remix](https://remix.run/docs/en/1.19.2/guides/typescript)
- [Gatsby](https://www.gatsbyjs.com/docs/how-to/custom-configuration/typescript/)
- [Expo](https://docs.expo.dev/guides/typescript/)

#### 在现有React项目中添加TypeScript

使用下面命令安装最新版本的 React 类型定义：

```js
npm install @types/react @types/react-dom
```

然后在 `tsconfig.json` 中设置以下编译器选项：

1. 必须在 [`lib`](https://www.typescriptlang.org/tsconfig/#lib) 中包含 `dom`（注意：如果没有指定 `lib` 选项，默认情况下会包含 `dom`）。
2. [`jsx`](https://www.typescriptlang.org/tsconfig/#jsx) 必须设置为一个有效的选项。对于大多数应用程序，`preserve` 应该足够了。 如果你正在发布一个库，请查阅 [`jsx` 文档](https://www.typescriptlang.org/tsconfig/#jsx) 以选择合适的值。

#### 在React组件中使用TypeScript

> 每个包含 JSX 的文件都必须使用 `.tsx` 文件扩展名。这是一个 TypeScript 特定的扩展，告诉 TypeScript 该文件包含 JSX。

使用 TypeScript 编写 React 与使用 JavaScript 编写 React 非常相似。与组件一起工作时的关键区别是，你可以为组件的 props 提供类型。这些类型可用于正确性检查，并在编辑器中提供内联文档。

以 [快速入门](https://react.docschina.org/learn) 指南中的 [`MyButton` 组件](https://react.docschina.org/learn#components) 为例，我们可以为按钮的 `title` 添加一个描述类型：

```tsx
function MyButton({ title }: { title: string }) {
  return (
    <button>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>欢迎来到我的应用</h1>
      <MyButton title="我是一个按钮" />
    </div>
  );
}
```

这种内联语法是为组件提供类型的最简单方法，但是一旦你开始描述几个字段，它可能变得难以管理。相反，你可以使用 `interface` 或 `type` 来描述组件的 props：

```tsx
interface MyButtonProps {
  /** 按钮文字 */
  title: string;
  /** 按钮是否禁用 */
  disabled: boolean;
}

function MyButton({ title, disabled }: MyButtonProps) {
  return (
    <button disabled={disabled}>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton title="我是一个禁用按钮" disabled={true}/>
    </div>
  );
}
```

描述组件 props 的类型可以根据需要变得简单或复杂，但它们应该是使用 `type` 或 `interface` 描述的对象类型。你可以在 [对象类型](https://www.typescriptlang.org/docs/handbook/2/objects.html) 中了解 TypeScript 如何描述对象，但你可能还对使用 [联合类型](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) 描述可以是几种不同类型之一的 prop，以及在 [从类型创建类型](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) 指南中参考更高级的用例。

#### Hooks示例

来自 `@types/react` 的类型定义包括内置的 Hook，因此你可以在组件中使用它们，无需任何额外设置。它们是根据你在组件中编写的代码构建的，所以你会得到很多 [类型推断](https://www.typescriptlang.org/docs/handbook/type-inference.html)，并且理想情况下不需要处理提供类型的细节。

但是，我们可以看一下如何为 Hook 提供类型的几个示例。

##### useState

[`useState` Hook](https://react.docschina.org/reference/react/useState) 会重用作为初始 state 传入的值以确定值的类型。例如：

```jsx
// 推断类型为 "boolean"
const [enabled, setEnabled] = useState(false);
```

这将为 `enabled` 分配 `boolean` 类型，而 `setEnabled` 将是一个接受 `boolean` 参数的函数，或者返回 `boolean` 的函数。如果你想为 state 显式提供一个类型，你可以通过为 `useState` 调用提供一个类型参数来实现：

```jsx
// 显式设置类型为 "boolean"
const [enabled, setEnabled] = useState<boolean>(false);
```

在这种情况下，这并不是很有用，但是当你有一个联合类型时，你可能想要提供一个 `type`。例如，这里的 `status` 可以是几个不同的字符串之一：

```jsx
type Status = "idle" | "loading" | "success" | "error";

const [status, setStatus] = useState<Status>("idle");
```

或者，如 [选择 state 结构原则](https://react.docschina.org/learn/choosing-the-state-structure#principles-for-structuring-state) 中推荐的，你可以将相关的 state 作为一个对象分组，并通过对象类型描述不同的可能性：

```jsx
type RequestState =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success', data: any }
  | { status: 'error', error: Error };

const [requestState, setRequestState] = useState<RequestState>({ status: 'idle' });
```

##### useReducer

[`useReducer`](https://react.docschina.org/reference/react/useReducer) 是一个更复杂的 Hook，它接受一个 reducer 函数和一个初始 state 作为参数，并将从初始 state 推断出 reducer 函数的类型。你可以选择性地为 `useReducer` 提供类型参数以为 state 提供类型。但是更高的做法仍然是在初始 state 上添加类型：

```tsx
import {useReducer} from 'react';

interface State {
   count: number 
};

type CounterAction =
  | { type: "reset" }
  | { type: "setCount"; value: State["count"] }

const initialState: State = { count: 0 };

function stateReducer(state: State, action: CounterAction): State {
  switch (action.type) {
    case "reset":
      return initialState;
    case "setCount":
      return { ...state, count: action.value };
    default:
      throw new Error("Unknown action");
  }
}
```

我们在几个关键位置使用了 TypeScript：

- `interface State` 描述了 reducer state 的类型。
- `type CounterAction` 描述了可以 dispatch 至 reducer 的不同 action。
- `const initialState: State` 为初始 state 提供类型，并且也将成为 `useReducer` 默认使用的类型。
- `stateReducer(state: State, action: CounterAction): State` 设置了 reducer 函数参数和返回值的类型。

为 `useReducer` 提供类型参数的更明确的替代方法是在 `initialState` 上设置类型：

```tsx
import { stateReducer, State } from './your-reducer-implementation';

const initialState = { count: 0 };

export default function App() {
  const [state, dispatch] = useReducer<State>(stateReducer, initialState);
}
```

##### useContext

[`useContext`](https://react.docschina.org/reference/react/useContext) 是一种无需通过组件传递 props 而可以直接在组件树中传递数据的技术。它是通过创建 provider 组件使用，通常还会创建一个 Hook 以在子组件中使用该值。

从传递给 `createContext` 调用的值推断 context 提供的值的类型：

```tsx
import { createContext, useContext, useState } from 'react';

type Theme = "light" | "dark" | "system";
const ThemeContext = createContext<Theme>("system");

const useGetTheme = () => useContext(ThemeContext);

export default function MyApp() {
  const [theme, setTheme] = useState<Theme>('light');

  return (
    <ThemeContext.Provider value={theme}>
      <MyComponent />
    </ThemeContext.Provider>
  )
}

function MyComponent() {
  const theme = useGetTheme();

  return (
    <div>
      <p>当前主题：{theme}</p>
    </div>
  )
}
```

当你没有一个合理的默认值时，这种技术是有效的，而在这些情况下，`null` 作为默认值可能感觉是合理的。但是，为了让类型系统理解你的代码，你需要在 `createContext` 上显式设置 `ContextShape | null`。

这会导致一个问题，你需要在 context consumer 中消除 `| null` 的类型。我们建议让 Hook 在运行时检查它的存在，并在不存在时抛出一个错误：

```tsx
import { createContext, useContext, useState, useMemo } from 'react';

// 这是一个简单的示例，但你可以想象一个更复杂的对象
type ComplexObject = {
  kind: string
};

// 上下文在类型中创建为 `| null`，以准确反映默认值。
const Context = createContext<ComplexObject | null>(null);

// 这个 Hook 会在运行时检查 context 是否存在，并在不存在时抛出一个错误。
const useGetComplexObject = () => {
  const object = useContext(Context);
  if (!object) { throw new Error("useGetComplexObject must be used within a Provider") }
  return object;
}

export default function MyApp() {
  const object = useMemo(() => ({ kind: "complex" }), []);

  return (
    <Context.Provider value={object}>
      <MyComponent />
    </Context.Provider>
  )
}

function MyComponent() {
  const object = useGetComplexObject();

  return (
    <div>
      <p>Current object: {object.kind}</p>
    </div>
  )
}
```

##### useMemo

[`useMemo`](https://react.docschina.org/reference/react/useMemo) 会从函数调用中创建/重新访问记忆化值，只有在第二个参数中传入的依赖项发生变化时，才会重新运行该函数。函数的类型是根据第一个参数中函数的返回值进行推断的，如果希望明确指定，可以为该 Hook 提供一个类型参数以指定函数类型。

```tsx
// 从 filterTodos 的返回值推断 visibleTodos 的类型
const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
```

##### useCallback

[`useCallback`](https://react.docschina.org/reference/react/useCallback) 会在第二个参数中传入的依赖项保持不变的情况下，为函数提供相同的引用。与 `useMemo` 类似，函数的类型是根据第一个参数中函数的返回值进行推断的，如果希望明确指定，可以为这个 Hook 提供一个类型参数以指定函数类型。

```jsx
const handleClick = useCallback(() => {
  // ...
}, [todos]);
```

当在 TypeScript 严格模式下，使用 `useCallback` 需要为回调函数中的参数添加类型注解。这是因为回调函数的类型是根据函数的返回值进行推断的——如果没有参数，那么类型就不能完全理解。

根据自身的代码风格偏好，你可以使用 React 类型中的 `*EventHandler` 函数以在定义回调函数的同时为事件处理程序提供类型注解：

```tsx
import { useState, useCallback } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  const handleChange = useCallback<React.ChangeEventHandler<HTMLInputElement>>((event) => {
    setValue(event.currentTarget.value);
  }, [setValue])

  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>值： {value}</p>
    </>
  );
}
```

#### 常用类型

当逐渐适应 React 和 TypeScript 的搭配使用后, 可以尝试阅读 `@types/react`，此库提供了一整套类型。你可以在 [DefinitelyTyped 的 React 目录中](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react/index.d.ts) 找到它们。我们将在这里介绍一些更常见的类型。

#### DOM事件

在 React 中处理 DOM 事件时，事件的类型通常可以从事件处理程序中推断出来。但是，当你想提取一个函数以传递给事件处理程序时，你需要明确设置事件的类型。

```jsx
import { useState } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  function handleChange(event: React.ChangeEvent<HTMLInputElement>) {
    setValue(event.currentTarget.value);
  }

  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>值： {value}</p>
    </>
  );
}
```

React 类型中提供了许多事件类型 —— 完整列表可以在 [这里](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/b580df54c0819ec9df62b0835a315dd48b8594a9/types/react/index.d.ts#L1247C1-L1373) 查看，它基于 [DOM 的常用事件](https://developer.mozilla.org/zh-CN/docs/Web/Events)。

当你需要确定某个类型时，可以先将鼠标悬停在你使用的事件处理器上，这样可以查看到事件的具体类型。

当你需要使用不包含在此列表中的事件时，你可以使用 `React.SyntheticEvent` 类型，这是所有事件的基类型。

#### 子元素

描述组件的子元素有两种常见方法。第一种是使用 `React.ReactNode` 类型，这是可以在 JSX 中作为子元素传递的所有可能类型的并集：

```jsx
interface ModalRendererProps {
  title: string;
  children: React.ReactNode;
}
```

这是对子元素的一个非常宽泛的定义。第二种方法是使用 `React.ReactElement` 类型，它只包括 JSX 元素，而不包括 JavaScript 原始类型，如 string 或 number：

```jsx
interface ModalRendererProps {
  title: string;
  children: React.ReactElement;
}
```

注意，你不能使用 TypeScript 来描述子元素是某种类型的 JSX 元素，所以你不能使用类型系统来描述一个只接受 `<li>` 子元素的组件。

你可以在这个 [TypeScript playground](https://www.typescriptlang.org/play?#code/JYWwDg9gTgLgBAJQKYEMDG8BmUIjgIilQ3wChSB6CxYmAOmXRgDkIATJOdNJMGAZzgwAFpxAR+8YADswAVwGkZMJFEzpOjDKw4AFHGEEBvUnDhphwADZsi0gFw0mDWjqQBuUgF9yaCNMlENzgAXjgACjADfkctFnYkfQhDAEpQgD44AB42YAA3dKMo5P46C2tbJGkvLIpcgt9-QLi3AEEwMFCItJDMrPTTbIQ3dKywdIB5aU4kKyQQKpha8drhhIGzLLWODbNs3b3s8YAxKBQAcwXpAThMaGWDvbH0gFloGbmrgQfBzYpd1YjQZbEYARkB6zMwO2SHSAAlZlYIBCdtCRkZpHIrFYahQYQD8UYYFA5EhcfjyGYqHAXnJAsIUHlOOUbHYhMIIHJzsI0Qk4P9SLUBuRqXEXEwAKKfRZcNA8PiCfxWACecAAUgBlAAacFm80W-CU11U6h4TgwUv11yShjgJjMLMqDnN9Dilq+nh8pD8AXgCHdMrCkWisVoAet0R6fXqhWKhjKllZVVxMcavpd4Zg7U6Qaj+2hmdG4zeRF10uu-Aeq0LBfLMEe-V+T2L7zLVu+FBWLdLeq+lc7DYFf39deFVOotMCACNOCh1dq219a+30uC8YWoZsRyuEdjkevR8uvoVMdjyTWt4WiSSydXD4NqZP4AymeZE072ZzuUeZQKheQgA) 中查看 `React.ReactNode` 和 `React.ReactElement` 的示例，并使用类型检查器进行验证。

#### 样式属性

当在 React 中使用内联样式时，你可以使用 `React.CSSProperties` 来描述传递给 `style` 属性的对象。这个类型是所有可能的 CSS 属性的并集，它能确保你传递给 `style` 属性的是有效的 CSS 属性，并且你能在编辑器中获得样式编码提示。

```jsx
interface MyComponentProps {
  style: React.CSSProperties;
}
```

#### 更多学习资源

本指南已经介绍了如何在 React 中使用 TypeScript 的基础知识，但还有更多内容等待学习。官网中的单个 API 页面或许包含了如何与 TypeScript 一起使用它们的更深入的说明。
文档中的各个 API 页面可能会包含更深入的说明，介绍如何在 TypeScript 中使用它们。

我们推荐以下资源：

- [TypeScript 官方文档](https://www.typescriptlang.org/docs/handbook/) 涵盖了大多数关键的语言特性。

- [TypeScript 发布笔记](https://devblogs.microsoft.com/typescript/) 深入介绍了每一个新特性。

- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/) 是一个社区维护的，用于在 React 中使用 TypeScript 的速查表，涵盖了许多有用的边界情况，并提供了比本文更广泛全面的内容。

- [TypeScript Community Discord](https://discord.com/invite/typescript) 是一个提问并获得有关 TypeScript 和 React issues 帮助的好地方。

### React开发者工具

使用 React 开发者工具检查 React [components](https://react.docschina.org/learn/your-first-component)，编辑 [props](https://react.docschina.org/learn/passing-props-to-a-component) 和 [state](https://react.docschina.org/learn/state-a-components-memory)，并识别性能问题。

##### 浏览器扩展

调试 React 构建的网站最简单的办法就是安装 React 开发者工具浏览器扩展。它可用于几种流行的浏览器：

- [安装 **Chrome** 扩展](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
- [安装 **Firefox** 扩展](https://addons.mozilla.org/zh-CN/firefox/addon/react-devtools/)
- [安装 **Edge** 扩展](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil)

现在，如果你访问一个用 **React 构建** 的网站，你将看到 **Components** 和 **Profiler** 面板。

![React Developer Tools extension](https://react.docschina.org/images/docs/react-devtools-extension.png)

#### Safari和其他浏览器

为其他浏览器（比如，Safari），安装 [`react-devtools`](https://www.npmjs.com/package/react-devtools) npm 包:

```jsx
# Yarn
yarn global add react-devtools

# Npm
npm install -g react-devtools
```

接下来从终端打开开发者工具：

```jsx
react-devtools
```

然后通过将以下 `<script>` 标签添加到你网站的 `<head>` 开头来连接你的网站：

```jsx
<html>
  <head>
    <script src="http://localhost:8097"></script>
```

现在在浏览器里刷新你的网站，你可以在开发者工具里查看它。

![React Developer Tools standalone](https://react.docschina.org/images/docs/react-devtools-standalone.png)

#### 移动端（React Native)

React 开发者工具同样可检查用 [React Native](https://reactnative.dev/) 构建的应用程序。

使用 React 开发者工具最简单的办法就是全局安装它：

```jsx
# Yarn
yarn global add react-devtools

# Npm
npm install -g react-devtools
```

接下来从终端打开开发者工具：

```jsx
react-devtools
```

它应该可以连接到任何正在运行的本地 React Native 应用程序。

> 如果几秒钟后开发者工具未连接，请尝试重新加载应用程序。

### 学习React

#### 描述用户界面

React 是一个用于构建用户界面（UI）的 JavaScript 库，用户界面由按钮、文本和图像等小单元内容构建而成。React 帮助你把它们组合成可重用、可嵌套的 *组件*。从 web 端网站到移动端应用，屏幕上的所有内容都可以被分解成组件。在本章节中，你将学习如何创建、定制以及有条件地显示 React 组件。

#### 你的第一个组件

React 应用是由被称为 **组件** 的独立 UI 片段构建而成。React 组件本质上是可以任意添加标签的 JavaScript 函数。组件可以小到一个按钮，也可以大到是整个页面。这是一个 `Gallery` 组件，用于渲染三个 `Profile` 组件：
