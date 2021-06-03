<template>
<div>
  <h1>todos</h1>
  <TodoList />
</div>
</template>
<script>
import TodoList from './components/TodoList.vue'
  export default {
    components:{
      TodoList
    }
  }
</script>
<style>


.whole{
  height: 800px;
   background:  rgba(230, 197, 197, 0.2);
}
*{
  box-sizing:border-box;

    
}
.container{
  max-width:700px;
  margin:75px auto;
  align-content:center;
 
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 30px;
  margin-bottom:-150px;
  font-size:24px;
 
}
h1{
  font-size: 100px;
  font-weight:lighter;
  color:rgba(201, 99, 99, 0.2);
  text-align:center;
  margin-top:-40px;
  padding-top:20px;
}
 p, a {
  color:rgba(0, 0 , 0 ,0.2);
  text-align: center;
  font: 11px verdana ;
  line-height: 20px;
  text-decoration:none;
  margin-top:200px;
}
a:hover
{
  text-decoration:underline;
  }

</style>