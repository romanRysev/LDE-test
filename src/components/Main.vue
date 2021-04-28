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
      let res = [{ label: "Выбрать всех" }]; // массив с результатом начинается с "Выбрать всех"
      let container = {}; // сюда будем складывать результаты поиска пользователей
      const listsObject = {}; // сделаем объект из lists, чтобы ускорить поиск т.к. искать в них будем часто и список может быть большим
      let path = [];

      const structures = this.setParentIdForStructures();

      this.apiData.lists.forEach((item) => {
        listsObject[item.id] = item;
        listsObject[item.id].paths = [];
      });

      /*this.apiData.structures.forEach((item) => {
        if (item.children.length > 0) {
          this.filterData(item.children, listsObject, item.id, container);
        }
      });*/
      this.filterDataRec(structures, listsObject, container, path);
      console.log(container);

      this.apiData.lists.forEach((list) => {
        res.push(...container[list.id]);
      });

      return res;
    },
  },
  methods: {
    setParentIdForStructures() {
      const data = this.apiData.structures.slice();
      function recursive(data, parentId, path) {
        data.forEach((item) => {
          item.parentId = parentId;
          item.path = path;
          const newPath = path.slice();
          newPath.push({ id: item.id, label: item.label });
          if (item.children && item.children.length > 0) {
            return recursive(item.children, item.id, newPath);
          } else {
            return;
          }
        });
        return data;
      }
      return recursive(data, null, []);
    },
    filterData(data, listsObject, parentId, container) {
      data.forEach((element) => {
        if (element.lists) {
          // Полагаю, что поле "lists" есть только у пользователей и у пользователей нет других "children"
          element.lists.forEach((listId) => {
            if (listsObject[listId]) {
              if (!container[listId]) {
                container[listId] = [listsObject[listId]];
              }
              container[listId].push(
                Object.assign({}, element, {
                  parentId: parentId,
                  currentListId: listId,
                }) // Добавляем копию т.к. иначе добавление пользователя в другой список перезапишет поле currentListId у всех экземпляров
              );
            }
            // Я не уверен, что правильно понял строчку в задании: "Массив элементов в итоговом списке должен получиться одноуровневым,
            // без вложенности.", ведь на скриншоте есть вложенность сотрудников в списки.
            // На всякий случай, чтобы сотрудники были вложены в списки, инструкции внутри этого forEach должны выглядеть так:
            //
            // if (listsObject[elem]) {
            //   if (!container[elem]) {
            //     container[elem] = listsObject[elem];
            //     container[elem].children = [];
            //   }
            //   container[elem].children.push(
            //     Object.assign({}, item, {
            //       parentId: parentId,
            //       currentListId: elem,
            //       }
            //     )
            //   );
            // }
          });
        } else if (element.children) {
          this.filterData(element.children, listsObject, element.id, container);
        } else {
          return;
        }
      });
    }, // Функция получилась "грязная" - модифицируется аргумент "container". В данном случае мне кажется, что это применимо
    // т.к. "container" создавался только для этого и не будет использоваться нигде больше.

    filterDataRec(data, listsObject, container) {
      data.forEach((element) => {
        if (element.lists) {
          element.lists.forEach((listId) => {
            if (listsObject[listId]) {
              if (!container[listId]) {
                container[listId] = listsObject[listId];
                container[listId].paths.push({
                  path: element.path,
                  elements: [],
                });
                container[listId].paths[
                  container[listId].paths.length - 1
                ].elements.push(
                  Object.assign({}, element, {
                    currentListId: listId,
                  })
                );
              } else {
                container[listId].paths;
              }

              /*container[listId].push(
                Object.assign({}, element, {
                  currentListId: listId,
                })
              );*/
            }
          });
        } else if (element.children) {
          this.filterDataRec(element.children, listsObject, container);
        } else {
          return;
        }
      });
    },
  },
};
</script>

<style scoped></style>
