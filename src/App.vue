<script setup>
  import Header from './components/Header.vue';
  import List from './components/List.vue';
  import Footer from './components/Footer.vue';
  import { ref,computed} from 'vue';
  //用v-model双向绑定数据，每组以对象记录待办事项名称和待办事项状态
  const obj=ref([])
  
  //计算已办事项数量
  const doneNum=computed(()=>{
    const num=ref(0);
    obj.value.forEach((item)=>{
      if(item.done){
        num.value++;
      }
    })
    return num.value;
  })
</script>

<template>
  <Header v-model="obj">
    <div v-if="obj.length==0" class="nonelist">暂无待办事项</div>
    <template v-else>
      
        <List v-for="list in obj">{{list.name}}</List>
           
        <Footer>已办事项{{doneNum}}/全部事项{{obj.length}}</Footer>
     
    </template>
  </Header>
</template>

<style scoped>
  .nonelist {
      text-align: center;
      font-size: 24px;
      font-weight: bold;
      color: #020000;
      margin-top: 10px;
  }
</style>
