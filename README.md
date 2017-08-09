# 安装
# 全局安装 vue-cli
## npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
## vue init webpack my-project
安装时会显示是否安装一些东西，都要选择否
# 安装依赖
## cd my-project
## npm install
## npm run dev
[vue官网](https://cn.vuejs.org/v2/guide/installation.html)

#### store.js

    const STORAGE_KEY='todos-vuejs'
    export default{
        fetch: function(){
            return JSON.parse(window.localStorage.getItem(STORAGE_KEY) || '[]')
        },
        save: function(items){
            window.localStorage.setItem(STORAGE_KEY,JSON.stringify(items))
        }
    }
    
#### App.js

    <template>
      <div id="app">
       <h1>{{title}}</h1>
       <h1 v-text="title"></h1>使用指令
       <h1 v-html="title"></h1>写入标签使用
       <input v-model="newItem" v-on:keyup.enter="addNew">
       <ul>
          <li v-for="item in items" v-bind:class="{finished:item.isFinishsd}" 
           v-on:click="toggleFinish(item)">
              {{item.label}}
          </li>
       </ul>
       <p>child tells me:{{ childWorld}}</p>
       <component-a msgfromfather='hi hi'
        v-on:child-tell-me-something='LinstnTOMyGirl'
       ></component-a>
      </div>
    </template>
    
    <script>
    import Store from "./store"
    import ComponentA from "./components/componentA"
    
    
    export default {
      name: 'hello',
      data () {
          return {
            title:'<span>?</span>this is a todo list',
              items:[
                {
                  label:'coding',
                  isFinishsd:true
                },
                {
                  label:'walking',
                  isFinishsd:true
                }
              ],
            liClass:'thisisLiClass',
            newItem:'',
            childWorld:''
          }
      },
    
      components: { ComponentA },
    
      watch: {
        items:{
          handler: function (items) {
            Store.save(items)
          },
            deep: true
          } 
        },
    
    methods:{
        toggleFinish:function(item){
            item.isFinishsd=!item.isFinishsd
        },
        addNew:function(){
          this.items.push({
              label:this.newItem,
              isFinishsd:false
          })
          this.newItem=''
        },
      LinstnTOMyGirl:function(msg){
          this.childWorld=msg;
      }
    
      }
    }
    </script>
    
    <style>
    .finished{
      text-decoration:underline;
    }
    #app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
      margin-top: 60px;
    }
    </style>
    
#### componentA.js

    <template>
    <div class="hello">
        <h1>{{msg}}</h1>
        <h1>{{msgfromfather}}</h1>
        <button v-on:click="onClickMe">Click!</button>
    </div>
    </template>
    
    <script>
        export default{
            data:function(){
                return{
                    msg:'hello from component a!'
                }
            },
            props:['msgfromfather'],
            methods:{
                onClickMe:function(){
                    console.log(this.msgfromfather); 
                    this.$emit('child-tell-me-something',this.msg);
                }
            }
    
        }
    </script>
    
    <style>
        .hello{
            color:green;
        }
    </style>

![项目目录](https://github.com/ss9931/vue.js/blob/master/1.png)
![效果图](https://github.com/ss9931/vue.js/blob/master/GIF.gif)

# vue.js
- 是new一个vue对象吧（设置属性）
- 重要选项
1. methods
2. watch
3. data
- 模板指令
1. v-text，v-html，{{}}...(常用)
- 控制模板隐藏
1. v-if，v-show
- 事件绑定
1. v-on，简写@
- 绑定属性
1. v-bind
- 如何划分组件
1. 功能模块
- select
- pagenation
- 如何划分组件
2. 页面区域
- header
- footer
- sidear
- 自定义事件
1. Son()监听事件
2. Semit()在它上面触发
3. Sdispatch()派发事件，事件沿着文链冒泡
4. Sbroadcast()广播插件，事件向下给所有后代