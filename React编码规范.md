> 参考阿里巴巴 Java 规范以及 Airbnb JS 规范

## Eslint + Prettier 配置

建议使用 `@umijs/fabric` 配置，具体使用如下：

1. 安装 `@umijs/fabric` `eslint-plugin-react-hooks`

   ```bash
   yarn add @umijs/fabric eslint-plugin-react-hooks -D
   ```

2. 配置`.eslintrc.js`文件

   ```javascript
   module.exports = {
     extends: [require.resolve('@umijs/fabric/dist/eslint')],
     plugins: ['react-hooks'],
     rules: {
       'react-hooks/rules-of-hooks': 'error', // 检查 Hook 的规则
       'react-hooks/exhaustive-deps': 'warn', // 检查 effect 的依赖
     },
   };
   ```

3. 配置`.prettierrc.js`文件

   ```javascript
   const fabric = require('@umijs/fabric');

   module.exports = {
     ...fabric.prettier,
   };
   ```

### 编辑器配置

1. VS Code

   - 安装 `Prettier` 插件
     ![Perttier插件]('./images/img2.png')
   - 设置保存时格式化
     ![Setting]('./images/img1.png')
   - 设置默认格式化工具为 `Prettier`

2. WebStorm
   - 安装 `Prettier` 插件
     ![Perttier插件]('./images/img3.png')
   - 设置 File Watchers
     ![Setting]('./images/img4')
   - 设置自动开启 eslint
     ![auto]('./images/img5.png')

## 编码规范

### 语法

1. 【强制】必须使用 **ES6** 或更高级语法

#### 命名

1. 【强制】使用 jsx 作为组件扩展名

2. 【强制】组件文件名使用*帕斯卡命名法*，不允许使用中文拼音，以英文直译，尽量明确表达该组件的用处。类名与文件名一致（如果是文件夹下的根组件，建议使用 index.jsx 命名）。

   ```jsx
   // 搜索树组件
   SearchTree.jsx

   class SearchTeee extend React.Component {}

   export default SearchTree;
   ```

3. 【强制】项目 src 文件树结构

   ```text
   pages    // 页面
   services   // 与服务端交互
   layouts   // 布局组件
   components  // 通用组件
   utils    // 公用方法
   assets   // 静态资源 如logo文件
   config   // 配置文件，如 router.config.js
   ```

4. 【强制】变量、方法名、类名一律不允许出现 **下划线** 开头或结尾

   - 如果后端返回的数据都是下滑线可以理解

5. 【强制】常量全部大写，单词间使用**下划线**相连。不要怕名字长，力求明确表达该常量的意义。

   ```javascript
   const SET_USER_INFO = 'set user info'; // 设置用户信息
   const CLEAR_USER_INFO = 'clear user info'; // 清除用户信息
   ```

6. 【强制】组件属性使用 **驼峰式命名**

   ```jsx
   <Component title="组件" subTitle="子项名称" extraTitle="其他名称" />
   ```

#### 美观易读

1. 【强制】组件属性对齐

   ```jsx
   // 单属性不换行
   <Component title="组件" />

   // 多个属性视情况换行
   <Component
      title="组件"
      subTitle="子项名称"
      extraTitle="其他名称"
   />
   ```

2. 【强制】自闭合标签之前留一个空格

   ```jsx
   <Component />
   ```

3. 【强制】import 引入大括号内左右空格

   ```javascript
   import { Button, Card } from 'antd';
   ```

4. 【建议】jsx 语法中 retrun 加括号

   ```jsx
   render(){
      return (
         <div>Hello,World!</div>
      )
   }

   const LoadingRender = (
      <Spin spinning={true} />
   )

   const functionRender = () => (
      <div>
         <p>Hello,World!</p>
      </div>
   )
   ```

5. 【强制】没有子组件的父组件必须使用自闭合方式

6. 【强制】constructor 内部必须传入 props 参数

   ```jsx
   constructor(props) {
      super(props);
   }
   ```

7. 【建议】如果组件没有依赖 props，state 单独声明

```jsx
class Page extends React.Component {
  state = {
    loading: true,
  }

  render() {
    return ...
  }
}
```

#### 写法

1. 【强制】没有内部状态需要维护的组件必须使用函数式组件

   ```jsx
   const PageHeader = ({ title }) => {
     return (
       <div>
         <h1>{title}</h1>
       </div>
     );
   };

   export default PageHeader;
   ```

2. 【建议】在 class 组件内部创建函数，提前在构造函数中绑定，不要在 jsx 中绑定。(或者直接使用箭头函数声明)

   ```jsx
   class HomePage extends React.Component {
     constructor(props) {
       super(props);
       this.handleSubmit = this.handleSubmit.bind(this);
     }

     // 函数声明
     handleSubmit() {
       // ...
     }

     // 箭头函数声明
     handleSubmit = () => {
       // ...
     };

     render() {
       return <Form onSubmit={this.handleSubmit}>...</Form>;
     }
   }

   export default HomePage;
   ```

3. 【建议】尽量不使用内联样式，写在 css 文件中

   ```jsx
   // bad
   <span style={{ textAlign: "center" }}> Hello,World! </span>

   // good
   .text-center {
      text-align: center;
   }
   <span className="text-center"> Hello,World! </span>
   ```

4. 【强制】需要同时返回多个组件时，不要在外部增加其他无意义的标签，使用数组标识或空标签（`React.Fragment`）。

   ```jsx
   // 空标签
   return (
     <>
       <A />
       <B />
       <C />
     </>
   );

   // 数组标识
   return [<A key="a" />, <B key="b" />, <C key="c" />];
   ```

5. 【建议】jsx 中使用双引号

   ```jsx
   <Component title="Don't do this" />
   ```

6. 【建议】字符串拼接使用 ES6 语法方式，避免直接相加

   ```jsx
   const str = `早安，${name}，祝你开心每一天！`;
   ```

7. 【建议】数据共享

   - 关于 redux 或其他：当你觉得应该用 redux 的时候就用 redux。
   - 关于为什么要用 redux 有疑问可以精读：[React Hooks 是不能替代 Redux 的](https://www.infoq.cn/article/k1Zb7ahbRmlrwDmkb6wY)
   - 如果多个组件都需要一份从后端获取的数据(可读类型)，不建议每个组件都调用一次接口，可以思考如何共享这份数据。

8. 【强制】判断数值是否相等使用三等号 "==="，判断为 null 或者 undifine 使用双等号 "=="

9. 【建议】多个判断条件（三个以上）使用 switch 或其他方法

   ```javascript
   function get(type) {
      if (type === "1"){
         return;
      }

      if (type === "2"){
         return;
      }

      if (type === "3"){
         return;
      }

      return ...
   }

   function get(type) {
      switch (type) {
      case "1":
         return ;
      case "2":
         return ;
      case "3":
         return ;
      default:
         return ;
      }
   }
   ```

10. 【强制】a 标签使用 target 属性时，要同时增加 rel 属性。

    ```jsx
    <a href="https://baidu.com" target="_blank" rel="noopener noreferrer">
      百度
    </a>
    ```

11. 【强制】a 标签的 href 属性不允许使用"javascrpt"关键字，使用 onClick 事件代替，或使用其他标签

    ```jsx
    // bad
    <a href="javascript:;">操作</a>

    // good
    <span className="action-text" onClick={this.action}>
     操作
    </span>
    ```

### 语义化

1. 【建议】各种循环的用法

   - 遍历但不需要返回值使用 `forEach()`
   - 遍历并且需要返回值使用 `map()`
   - 需要找到数组中的一项数据使用 `find()`
   - 需要找到数组中的一项数据的索引使用 `findIndex()`
   - 需要知道数据中是否拥有该数据使用 `some()`
   - 数组中的每一项是否都满足条件使用 `every()`
   - 找到数组中的某一项值并修改 使用 `for of` - 提前结束循环并且不需要返回值

#### 其他

1. 【强制】同一项目中不允许使用多个技术栈，React 就必须全部使用 React，不能掺杂 Vue 代码。使用 JS 就全部使用 JS，不能掺杂 TS。
2. 【建议】关于 TypeScript，如果是新启动的前端项目，在可以完整运用 TS 时可以使用 TS。
3. 【强制】第 2 点续：使用 TS，就必须参照 TS 开发规范。不能充斥着各种 any。
4. 【建议】不要为了节省一点点的性能而把代码写的特别复杂。
