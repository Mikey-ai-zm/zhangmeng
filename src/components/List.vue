<script setup>
    import { inject,getCurrentInstance,computed} from 'vue'
    //注入父组件的Header
    const parent=inject('Header')

    //将该实例添加到父组件的children数组中，并获取当前组件的Index
    const instance=getCurrentInstance()
    parent.addChild(instance)
    const index=parent.children.value.indexOf(instance)

    const flag=computed(()=>parent.props.modelValue[index].done)

    //切换任务完成状态
    const toggleFlag = () => {
        parent.props.modelValue[index].done=!flag.value
    }
    
    //删除单组件
    const deleteItem = ()=>{
        parent.removeItem(index)
    }


</script>

<template>
    <div class="box">
        <span class="left" :class="{done:flag}">
            <span class="squre" @click="toggleFlag"><span v-if="flag">√</span></span>
            <span class="text"><slot></slot></span>
        </span>
        <button @click="deleteItem">删除</button>
    </div>
</template>

<style scoped>
div{
    margin: 0;
    padding: 10px 30px;
    display: flex;
    
    justify-content: space-between;
    align-items: center;
}
div:hover{
    background-color: #e6e4e4;
}
.left{
    display: flex;
    align-items: center;
}
.squre{
    display: inline-block;
    width: 15px;
    height: 15px;
    border: 1px solid grey;
    border-radius: 5px;
    margin-right: 10px;
    align-self: center;
    
}
.squre:hover,button:hover{
    cursor: pointer;
}
button{
    padding: 2px 15px;
}

.done .squre{
    content: '√';
}
.done .text{
    text-decoration: line-through;
    color: red;
}
</style>
