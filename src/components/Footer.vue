<script setup>
    import { ref,inject,computed } from 'vue'
    
    const parent=inject('Header')
    const flag=computed(()=>parent.allDone.value)
    console.log(flag.value)

    //点击时修改数组每个元素的状态
    function handleClick(){
        
        if(flag.value){
            parent.props.modelValue.forEach(item=>item.done=false)
        }else{
            parent.props.modelValue.forEach(item=>item.done=true)
        }
    }
   
</script>

<template>
    <div class="footer">
        <span class="left">
            <span @click="handleClick" class="squre"><span v-if="flag">√</span></span>
            <span><slot></slot></span>
        </span>
        <span class="right">
            <button @click="parent.clearDone()">清除已办事项</button>
            <button @click="parent.clearAll()">清除所有事项</button>
        </span>
    </div>
</template>

<style scoped>
.footer{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 30px;
    margin: 0;
    margin-top: 20px;
    background-color: #08b7ff;
}
.left{
    display: flex;
    align-items: center;
}
.squre{
    display: inline-block;
    width: 18px;
    height: 18px;
    
    border: 1px solid rgb(58, 58, 58);
    border-radius: 5px;
    margin-right: 10px;
    cursor: pointer;
}
button:hover{
    cursor: pointer;
}
button{
    padding: 2px 15px;
    margin-left: 10px;
}
</style>