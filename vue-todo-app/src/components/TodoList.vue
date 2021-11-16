<template>
  <div>
      <ul>
          <li v-for="(todoItem, index) in todoItems" :key="todoItem.item" class="shadow">
            <i class="fas fa-check checkBtn" v-bind:class="{checkBtnCompleted: todoItem.completed}" 
            v-on:click="toggleComplete(todoItem)"></i>
            <span v-bind:class="{textCompleted: todoItem.completed}">{{ todoItem.item }}</span>
            <span class="removeBtn" v-on:click="removeTodo(todoItem, index)">
                <i class="fas fa-trash-alt"></i>
            </span>
          </li>
      </ul>
  </div>
</template>

<script>
export default {
    data: function(){
        return{
            todoItems: []
        }
    },
    methods: {
        removeTodo: function(todoItem, index){
            // console.log(todoItem, index);
            localStorage.removeItem(index); //localStorage 에서 삭제
            this.todoItems.splice(index, 1); //화면단에서 삭제
        },
        toggleComplete: function(todoItem){
            todoItem.completed = !todoItem.completed;
            //localStorage에 삭제 후 다시 등록해야함(갱신)
            localStorage.removeItem(todoItem.item);
            localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
        }
    },
    created: function(){
        if(localStorage.length > 0){
            for(var i = 0; i < localStorage.length; i++){
                if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
                    // JSON.parse(localStorage.getItem(localStorage.key(i))); //객체로(value 추출)
                    this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
                    // this.todoItems.push(localStorage.key(i));
                }
            }
        }
    }
}
</script>

<style scoped>
ul{
    list-style: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
}
li{
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: #fff;
    border-radius: 5px;
}
.removeBtn{
    margin-left: auto;
    color: #de4343;
}
.checkBtn{
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
}
.checkBtnCompleted{
    color: #b3adad;
}
.textCompleted{
    text-decoration: line-through;
    color: #b3adad;
}

</style>