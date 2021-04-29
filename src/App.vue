<template>
  <div id="app">
    <List />
    <Form />
  </div>
</template>

<script>
import List from "./components/List.vue";
import Form from "@/components/Form.vue";
export default {
  name: "App",
  components: {
    List,
    Form,
  },
};
</script>

<style></style>
