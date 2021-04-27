<template>
  <div class="main">
    <p v-for="(item, id) in filteredData" :key="id">{{ item }}</p>
  </div>
</template>

<script>
import data from "@/new.json";
export default {
  data() {
    return {
      apiData: data,
    };
  },
  computed: {
    filteredData() {
      let res = [{ label: "Выбрать всех" }];
      let container = [];

      this.apiData.lists.forEach((item) => {
        res.push(item);
        if (item.id) {
          this.filterData(this.apiData.structures, item.id, item.id, container);
          res = res.concat(container);
          // Я не уверен, что правильно понял строчку в задании:
          // "Массив элементов в итоговом списке должен получиться
          // одноуровневым, без вложенности.", ведь на скриншоте
          // есть вложенность сотрудников в списки.
          //
          // На всякий случай, чтобы сотрудники были вложены в списки,
          // замените строку над комментарием на следующую строку :)
          // res[res.length - 1].children = container;
          container = [];
        }
      });

      return res;
    },
  },
  methods: {
    filterData(data, listId, parentId, container) {
      data.forEach((item) => {
        if (item.lists && item.lists.includes(listId + "")) {
          container.push(
            Object.assign(item, { parentId: parentId, currentListId: listId })
          );
        } else if (item.children) {
          this.filterData(item.children, listId, item.id, container);
        } else {
          return;
        }
      });
    },
  },
};
</script>

<style scoped></style>
