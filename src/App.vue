<template>
  <div>
    <section class="todoapp">
      <header class="header">
        <h1>todos</h1>
        <input 
          class="new-todo"
          autofocus
          autocomplete="off"
          placeholder="What needs to be done?"
          v-model="text"
          @keyup.13="onEnter"/>
      </header>
      <section class="main">
        <input
          id="toggle-all"
          class="toggle-all"
          type="checkbox"
          v-model="checkeds"
          />
        <label for="toggle-all" v-show="lists.length>0"></label>
          <router-view 
            :lists="lists" 
            @check="check"
            @del="del"
            @editBtn="editBtn"
            @save="save"/>
      </section>
      <footer class="footer"  v-show="lists.length>0">
        <span class="todo-count">
          <strong>{{ num }}</strong> {{ num > 1 ? 'items left' :'item left' }}
        </span>
        <ul class="filters">
          <li>
            <router-link to="/" tag="a">All</router-link>
          </li>
          <li>
            <router-link to="/Active" tag="a">Active</router-link>
          </li>
          <li>
            <router-link to="/Completed" tag="a" >Completed</router-link>
          </li>
        </ul> 
        <button class="clear-completed" @click="clear" v-show="num!=lists.length?true:false">
          Clear completed
        </button>
      </footer>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text:'',          // 文本框输入内容
      lists:[],          // 内容
      editCon:'',       // 修改内容
    }
  },
  methods: {
    onEnter(){
      if(this.text=='')return;
      this.lists.push({
        checked:false,   
        text:this.text,  
        isTrue:false    
      });
      this.text = '';
    },
    del(index){
      this.lists.splice(index,1);
    },
    check(index){
      this.lists[index].checked = !this.lists[index].checked;
    },
    clear(){
      for(var i=0;i<this.lists.length;i++){
        if(this.lists[i].checked){
          this.lists.splice(i,1);
          i-=1;
        }
      }
    },
    editBtn(index){
      this.editCon = this.lists[index].text;
      this.lists[index].isTrue = true;     
    },
    save(obj){
      this.lists[obj.index].text = obj.text ;
      this.lists[obj.index].isTrue = false;  
    },
  },
  computed:{
    num(){
      let total = 0;
      this.lists.forEach(ele=>{if(!ele.checked){total+=1;}});
      return total;
    },
    checkeds:{
        get(){return this.lists.every(item=>{return item.checked;})},
        set(val){this.lists.forEach((item)=>{item.checked = val;})}
    }
  }
}
</script>