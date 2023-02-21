<template>
    <div>
        <ul class="todo-list">
            <li class="todo" 
            v-for="(item,index) in lists" :key="index" 
            :class="{'completed':item.checked,'editing':item.isTrue}">
            <div class="view">
                <input class="toggle" type="checkbox" @change="check(index)" :checked="item.checked"/>
                <label @dblclick="editBtn(index)">{{ item.text }}</label>
                <button class="destroy" @click="del(index)"></button>
            </div>
            <input class="edit" id="inputId"  type="text" v-model="editCon" @change="save(index)"/>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props:['lists'],
    data() {
        return {
            editCon:'',   // 修改框
        }
    },
    methods: {
        // 单选
        check(index){
            this.$emit('check',index);
        },
        // 修改
        editBtn(index){
            this.editCon = this.lists[index].text;
            this.$emit('editBtn',index);
        },
        // 修改i保存
        save(index){
            this.$emit('save', {index,text:this.editCon});
        },
        // 删除
        del(index){
            this.$emit('del', index);
        }
    },
}
</script>