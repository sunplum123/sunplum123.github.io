---
title: React简单入门
date: 2017-04-05 19:18:56
categories: 前端
tags:
 -react
---
#### 1、React的独特语法JSX
```html
ReactDOM.render(
    <span>1111</span>,
    document.getElementById("example");
)
```

>ReactDOM有一个render方法，接受两个参数，一个参数是虚拟的Dom,另一个是真实的Dom。此方法：相当于把虚拟Dom放入真实的Dom下面   

#### 2、React的组件Component
######  （1）Component的基本语法
```html
class MyLabel extends React.Component{
    render(){
        return <h1>111111</h1>
    }
}
ReactDOM.render(<MyLabel/>,document.getElementById("#example"));
```
> 1、ES6 新写法，定义一个MyLabel的类，继承基类React.Component所有属性和方法。
2、自定义组件的首字母必须大写。MyLabel 不能写成myLabel，以便和原生类区分。
3、必须有render()方法，定义输出的样式。返回的虚拟DOM最外层只能有一个标签。
4、在ReactDOM.render()中接收的虚拟Dom可以是自定义组件的实例，实例必须是闭合标签<MyLabel/>或者<MyLabel></MyLabel>   

<!-- more -->
###### （2）Component的传输参数
```html
class MyLabel extends React.Component{
    render(){
        return <h1 style={{color:this.props.color}}>Hello World</h1>
    }
}
ReactDOM.render(<MyLabel color="red"/>,document.getElementById("example"));
```
> 组件内部通过this.props传输参数

###### （3）Compoent的状态
```html
class MyLabel extends React.Component{
    constructor(...args){
        super(...args);
        this.state = {
            name:"访问者"
        }
    }
    
    handleChange(e){
        let name = e.target.value;
        this.setState({
            name : name
        })
    }
    
    render(){
        return <div>
            <input type="text" onChange={this.handleChange.bind(this)} />
            <p>您好！{this.state.name}</p>
        </div>;
    }
}
ReactDOM.render(<MyLabel/>,document.getElementById("example"));
```
> 1、首先注意组件内部有个状态属性，state,如果想要初始化内部属性，请使用constructor构造函数初始化！   
2、有个setState()方法用来改变state属性。   
3、handleChange是自定义方法，换成其他名字皆可。event的来源应该来自组件自身内部

```javascript
// 进阶实例
class MyLabel extends React.Component{
    constructor(...args){
        super(...args);
        this.state = {
            isSingleClick:false,
            text:"World"
        }
    }
    handleClick(){
        let isClicked = !isSingleClick;
        this.setState({
            isSingleClick:!this.state.isSingleClick,
            text:isClicked?true:false
        })
    }    
    render(){
        return <h1 onClick={this.handleClick.bind(this)}>{"Hello "+this.state.text}}</h1>;
    }
}

ReactDOM.render(<MyLabel/>,document.getElementById("example"));

```

###### （4）Component的生命周期
```html
class MyLabel extends React.Component{
    constructor(...args){
        super(...args);
        this.state = {
            loading:true,
            data:null,
            error:null
        }
    }
    
    // 组件结束后执行的方法
    componentDidMount(){
        const url = 'https://api.github.com/search/repositories?q=javascript&sort=stars';
        $.getJSON().done(
            (value)=>this.setState({
                data:value,
                loading:false
            })
        ).error(
            (jqXHR,textState) => this.setState({
                error:jqXHR.status;
            })
        )
    }
    
    render(){
        let loading = this.state.loading;
        let text;
        
        if(loading){
            text = "loading";
        }else{
            text ="成功";
        }
        
        return <span>{text}</span>;
    }
}

```
**React为组件的生命周期提供了将近十几个钩子方法**
>- componentWillMount()：组件加载前调用
>- componentDidMount()：组件加载后调用
>- componentWillUpdate()：组件更新前调用
>- componentDidUpdate()：组件更新后调用
>- componentWillUnmount()：组件卸载前调用
>- componentWillReceiveProps()：组件接受新的参数时调用   

我们可以利用这些钩子，自动完成一些操作。

###### （5）React的组件库
比如[React-bootstrap](https://react-bootstrap.github.io/) 和
[ReCharts](https://react-bootstrap.github.io/)（图表组件库）

#### 3、React的核心思想 
view是state的输出
```javascript
view = f(state)
```
当state发生变化时，view也会发生相应的变化

> React的本质是将图形界面函数化   

```html
    // 仔细看懂此例子，测试通过
    const person = {
        name:"liyang",
        age:18
    }
    
    const App = ({person})=><h1>{person.name}</h1>;
    
    ReactDOM.render(<App />,document.body);
```
