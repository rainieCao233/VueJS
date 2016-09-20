<template>
  <div id="Todolist">
    <h1>{{title}}</h1>
    <input type="text" v-model="newItem" @keyup.enter="addNew">
    <ul>
      <li v-for="item in items" v-bind:class="{finished: item.isFinished}" v-on:click="toggleFinish(item)">
        {{item.label}}
      </li>
    </ul>
    <!-- 父与子、子与父进行通信 -->
    <p>child tells me : {{ childWords }}</p>
    <!-- <Component-a msgfromfather="woshishi" v-on:children-tell-me-something="ListenToMyBoy"></Component-a> -->
    <Component-a msgfromfather="woshishi"></Component-a>
  </div>
</template>

<script>
import Store from "./Store"
import ComponentA from "./components/componentA"

export default {
  data: function(){
    return {
      title:'this is a todo list',
      items: Store.fetch(),
      newItem:"",
      childWords:''
    }
  },
  components:{
    ComponentA
  },
  events:{
    "children-tell-me-something":function(msg){
      this.childWords = msg
    }
  },
  methods:{
    toggleFinish:function(item){
      item.isFinished = !item.isFinished;
    },
    addNew:function(){
      this.items.push({
        label: this.newItem,
        isFinished:false
      });
      this.newItem = "";
      this.$broadcast("onAddNew", this.items);
    },
    ListenToMyBoy:function(msg){
      this.childWords = msg
    }
  },
  watch:{
    "items":{
      handler:function(items){
        Store.save(items);
      },
      deep:true
    }
  }
}
</script>

<style>
h1{
  color: #42b983;
  width: 100%;
}
.finished{
  text-decoration: underline;
}
</style>
