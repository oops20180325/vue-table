<template>
  <div class="hello">
    <demo-grid :data='gridData' :columns='gridColumns'></demo-grid>
  </div>
</template>

<script>
 import demoGrid from './demo.vue'
export default {
  name: 'HelloWorld',
  data () {
    return {
      // table 
      //数据结构很重要 : id 是必须的 
      gridColumns: [ 'id','name', 'power'], 
      gridData: [ 
        { id:1, name: 'Chuck Norris', power: Infinity}, 
        {id:2, name: 'Bruce Lee', power: 9000 },
        {id:3, name: 'Jackie Chan', power: 7000 }, 
        { id:4,name: 'Bruce Lee', power: 9000},
        {id:5, name: 'Jackie Chan', power: 7000 }, 
        { id:6,name: 'Bruce Lee', power: 9000 }, 
        {id:7, name: 'tom', power: 800 },
        { id:8,name: 'jeyy', power: 1000 }, 
        { id:9,name: 'tony', power: 2000 }, 
        { id:10,name: 'marry', power: 3000 }, 
        { id:11,name:'kack', power: 3000 }, 
        {id:12, name: 'loon', power: 3000 }, 
        { id:13,name: 'lucy', power: 3000 }
      ]
    }
  },
  components:{
    demoGrid
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
