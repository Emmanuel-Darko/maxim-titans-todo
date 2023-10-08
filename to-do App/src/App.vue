<template>
  <div>
    <HeaderComponent />
    <TodoListComponent />
  </div>
</template>
<script>
import HeaderComponent from "./components/HeaderComponent.vue";
import TodoListComponent from "./components/TodoListComponent.vue";
export default {
  components: {
    HeaderComponent,
    TodoListComponent,
  },
};
</script>

<style scoped>
* {
  width: 50%;
  margin: auto;
}
</style>
