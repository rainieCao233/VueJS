<template>
  <div id="A">
    <h1>{{ msg }}</h1>
    <h1>{{ msgfromfather }}</h1>
    <button v-on:click="onClickMe">click!</button>
  </div>
</template>

<script>
export default{
  data:function(){
    return {
      msg:"hello from component A"
    }
  },
  props:["msgfromfather"],
  events:{
    "onAddNew":function(items){
      console.log(items)
    }
  },
  methods:{
    onClickMe:function(){
      console.log(this.msgfromfather);
      // this.$emit("children-tell-me-something", this.msg);
      this.$dispatch("children-tell-me-something", this.msg);
    }
  }
}
</script>

<style>

</style>
