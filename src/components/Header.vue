<script setup>
    import { ref,provide,computed } from 'vue'
    //用v-model绑定props.modelValue
    const props=defineProps({
        modelValue:{
            type:Array,
            default:()=>[]
        }
    })
    //收集孩子，并将孩子的状态随着modelValue变化而变化
    const children=ref([])
    const emit=defineEmits(['update:modelValue'])
    //绑定新增代办事项
    const newItem = ref('')
    //添加事项时触发  --向modelValue中添加新事项的名称和状态
    const addClick = () => {
        if (newItem.value.trim()) {
            props.modelValue.push({name:newItem.value.trim(),done:false})
            newItem.value = ''
        }
    }
    //用provide提供给list子组件，在子组件删除事项时触发该函数   --1.删除对应modelValue和children数组元素
    const removeItem = (index) => {
        props.modelValue.splice(index, 1)
        children.value.splice(index, 1)
    }

    //由modelValue计算是否全部事项已完成  --设为计算属性深度监听modelValue中各项的done属性
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

    
    //由provide提供给footer子组件，清除已办事件按钮触发该事件，
    //    
    const clearDone=()=>{
        //1.更新modelValue的值
        emit('update:modelValue',props.modelValue.filter((item)=>item.done==false))

        //2.并且更新children数组，以保证子组件的获取到的自己的Index值是对的
        const indexs=ref([])
        props.modelValue.forEach((item,index)=>{
            if(item.done==true){
                indexs.value.push(index)
            }
        })
        children.value=children.value.filter((item,index)=>{
            if(indexs.value.includes(index)){
                return true
            }
        })
    }
    //由provide提供给footer子组件，清除全部事件按钮触发该事件，更新modelValue和children的值为空
    const clearAll=()=>{
        emit('update:modelValue',[])
        children.value=[]
    }

    //provide给子组件，用于获取和修改modelValue的状态
    provide('Header',{
        props,//用于向子组件提供modelValue
        removeItem,//用于list子组件删除事项时触发该函数
        allDone,//用于footer判断全部事项已完成
        clearDone,//用于footer清除已办事件按钮触发该事件
        clearAll,//用于footer清除全部事件按钮触发该事件
        children,//用于list子组件获取自己的Index值
        addChild:function (instance){
            children.value.push(instance)
        },
    })
</script>

<template>
    <header>
        <h2 class="title">ToDoList</h2>
        <div>
            <input type="text" v-model.lazy="newItem">
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