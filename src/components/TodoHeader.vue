<template>
<div>
  <h1>Todo List</h1>
  <h2>{{ today }}</h2>
</div>
</template>

<script>
export default {
  data(){
    return{
      today: "",
    };
  },
  created() {
    const now = new Date();
    const month = now.getMonth() + 1;
    const date = now.getDate();
    const weekList = new Array("Sun", "Mon", "Tue", "Wed", "Thur", "Fri", "Sat");
    const week = weekList[now.getDay()];
    this.today = `${month}/${date} ${week}`;
  }
}
</script>

<style scoped>

</style>
