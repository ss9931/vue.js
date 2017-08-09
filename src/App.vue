<template>
  <div id="app">
   <h1 v-html="title"></h1>
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
