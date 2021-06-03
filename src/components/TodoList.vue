<template>
  <div class="main" >
       
      <input type="text" name="fname" class="todo-input" placeholder="What needs to be done?" v-model="newTodo" @keyup.enter="addTodo"/>
       <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
       <div v-for="todo in todosFiltered" :key="todo.id" class="todo-item"> 
       <div class="todo-item-left">
         
         <input type="checkbox" v-model="todo.completed">
         <div  v-if="!todo.editing" @click="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed}">{{todo.title}} </div>
         <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)"   v-focus>
        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
        
    </div>
      </transition-group>
    <div class="extra-container"><br><br>
      <div class="remain">{{ remaining }} items left </div> 
           <button :class="{active: filter == 'all'}" @click="filter = 'all'"> All</button> 
           <button :class="{active: filter == 'active'}" @click="filter='active'">Active</button>
           <button :class="{active: filter == 'completed'}" @click="filter ='completed'">Completed</button>
       <div>
            <button id="btn" v-if="ClearCompletedButton" @click="ClearCompleted">Clear Completed</button>
        </div>
       
       </div>
          
  </div>
  
</template>

<script>
export default {
  name: 'todo-list',
    data () {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache:'',
      completed: false,
      editing: false,
      filter:'all',
      todos:[]
    }
  },
  computed: {
    remaining(){
      return this.todos.filter(todo => !todo.completed).length 
    },
    anyRemaining(){
      return this.remaining !=0
    },
    todosFiltered(){
      if(this.filter == 'all'){
        return this.todos
      } else if(this.filter == 'active'){
        return this.todos.filter(todo => !todo.completed)
      }else if( this.filter =='completed'){
        return this.todos.filter(todo => todo.completed)
      }
        return this.todos
    },
    ClearCompletedButton() {
        return this.todos.filter(todo => todo.completed).length > 0
      }
  },
  directives: {
    focus:{
      inserted: function(el){
        el.focus()
      }
    }
  },
  methods:{
      addTodo(){
        if(this.newTodo.trim().length == 0){
          return
        }
        
         this.todos.push({
             id: this.idForTodo,
             title: this.newTodo,
             completed: false,
         }) 
          
          this.newTodo = ''
          this.idForTodo++
      },
      editTodo(todo){
        this.beforeEditCache = todo.title
        todo.editing = true
      },
      doneEdit(todo){
        if(todo.title.trim() == ''){
          todo.title = this.beforeEditCache
        }
        todo.editing = false
      },
      cancelEdit(todo){
        
        todo.title = this.beforeEditCache
        todo.editing = false
      },
      removeTodo(index){
        
         this.todos.splice(index, 1);
         
       
      },
      checkAllTodos() {
        this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    ClearCompleted(){
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >

.main{
  background-color:white;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 30px 0 rgba(0, 0, 0, 0.1);
  width:600px;
  margin: auto;
  }
.todo-input{
     height:60px;
    width:600px;
    margin-top:-50px;
    
    background-color:white;
    border:0.5px solid rgb(177, 170, 170, 0.2);
    text-align:left;
    font: 25px black;
    padding-left: 80px;
    box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.2), 0 4x 4px 0 rgba(0, 0, 0, 0.1);
}
    input[type="text"]:focus{
        outline: 0;
    
    
}
input[type="text"]::placeholder{
  opacity:0.5;
  color:rgb(167, 163, 163);
  font-style:italic calibry;
  font: weight 5px;
}
.todo-item{
    display: flex;
    align-items:center;
    justify-content:space-between;
    animation-duration: 0.10s; 
    border-bottom: 0.2px solid rgb(231, 219, 219, 0.5);
    padding: 25 px;
    font-size:26px;
    }
       
    .todo-item-left{
      display: flex;
      align-items: center;
      margin: 6px;
    }
    .todo-item-edit{
      padding: 10px;
      color: #2c3e50;
      width: 200px;
      border: 1px solid #ccc;
      font-family: 'avenir',Helvatica, Arial, sans-serif;
      outline:none;
      align-items:center;
      font-size:25px;
      
    }
    .remove-item{
       color:white;
              
      }
      
    .remove-item:hover,.remove-item:focus{
        cursor: pointer;
        float:right;
        position: relative;
        color: rgb(22, 14, 14);
        font-weight:lighter;
        font-size:28px;
        margin:0 30px 0 30px;
      }
     .todo-item-edit:focus{
        outline: none;
      }
    .completed{
      text-decoration: line-through;
      color: grey;
      
    }
    .extra-container {
      display: flex;
      align-items: center;
      padding: 5px;
      font-size: 16px;
      border-top:1px solid lightgrey;
      padding-top: 14px;
      margin-bottom: 14px;
      opacity:0.5;
      width:600px;
    }
    button{
      font-size: 14px;
      background-color: white;
      appearance: none;
      margin-left: 20px;
      border-style: none;
      align-items: center;
      transition:transform 100ms ease-in;
       
    }
    button:hover{
         background:white;
         cursor:pointer;
         transition:scale(1.07);
         }
         button:focus {
           outline: none;

         }
    .active {
      border: 1px solid rgb(241, 208, 203);

    }
    .fade-enter-active, .fade-leave-active {
      transition: opacity .2s;
    }
    .fade-enter, .fade-leave-to{
      opacity: 0;
    }
    .check{
     fill-opacity:0.2;
     
    }
    #btn{
       margin-left:75px;
    }
    #btn:hover{
    text-decoration:underline;
    cursor:pointer;
   
    }
    input[type="checkbox"]{
      opacity:0.01;
      
    }
    .remain{
      margin-right:80px;
      margin-left: 10px;
    }
   
</style>
