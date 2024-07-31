<script setup>
    import { ref,provide,computed } from 'vue'
    //用v-model绑定props.modelValue
    const props=defineProps({
        modelValue:{
            type:Array,
            default:()=>[]
        }
    })
    //绑定新增代办事项
    const newItem = ref('')
    //添加事项时触发
    const addClick = () => {
        if (newItem.value.trim()) {
            props.modelValue.push({name:newItem.value.trim(),done:false})
            newItem.value = ''
        }
    }
    //用provide提供给子组件，在子组件删除事项时触发该函数
    const removeItem = (index) => {
        props.modelValue.splice(index, 1)
    }
    const allDone=computed(()=>{
        const doneList=props.modelValue.filter((item)=>item.done==true)
        const flag=ref(false)
        if(doneList.length==props.modelValue.length){
            flag.value=true
        }else{
            flag.value=false
        }
        return flag.value
    })
    const emit=defineEmits(['update:modelValue'])
    const clearDone=()=>{
        emit('update:modelValue',props.modelValue.filter((item)=>item.done==false))
    }
    const clearAll=()=>{
        emit('update:modelValue',[])
    }
    provide('Header',{
        props,
        removeItem,
        allDone,
        clearDone,
        clearAll,
        children:[],//用于让子组件确定它在父组件中的位置，以获取和修改modelValue的状态
        addChild:function (instance){
            this.children.push(instance)
        },
    })
</script>

<template>
    <header>
        <h2 class="title">ToDoList</h2>
        <div>
            <input type="text" v-model="newItem">
            <button @click="addClick">添加</button>
        </div>
    </header>
    <slot></slot>
    <slot name="lists"></slot>
    <slot name="footer"></slot>
</template>

<style scoped>
    header{
        width: 100%;
        background-color: #0b87ff;
        color:#f0f8ff;

        text-align: center;
        padding: 10px;

        display: flex;
        flex-direction: column;
        justify-content: space-around;
    }
    .title{
        margin: 0;
        padding: 6px;
    }
    header div{
        display: flex;
        justify-content: center;
        align-items: center;
        
    }
    header input{
        
        width: 260px;
        height: 20px;
        padding: 5px;
        border: none;
        border-radius: 5px;
        margin-right: 10px;
    }
    header button{
        background-color:lightgray;
        color: black;
        font-size: 14px;
        border: none;
        border-radius: 5px;
        padding: 5px 10px;
        cursor: pointer;
    }
    
</style>