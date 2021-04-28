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

      const structures = this.setParentIdForStructures();

      this.apiData.lists.forEach((item) => {
        listsObject[item.id] = item;
      });

      /*this.apiData.structures.forEach((item) => {
        if (item.children.length > 0) {
          this.filterData(item.children, listsObject, item.id, container);
        }
      });*/
      container = this.filterDataWithLevels(structures);
      console.log(container);
      this.apiData.lists.forEach((list) => {
        res.push(list);
        const sourceTable = {};
        container.forEach((item) => {
          item.forEach((elem) => {
            if (Object.entries(elem.children).length > 0) {
              if (sourceTable[elem.parentId] !== undefined) {
                res.splice(
                  sourceTable[elem.parentId],
                  0,
                  ...[
                    { id: elem.id, label: elem.label, parentId: elem.parentId },
                    ...elem.children[list.id],
                  ]
                );
              } else {
                res.push({
                  id: elem.id,
                  label: elem.label,
                  parentId: elem.parentId,
                });
                res.push(...elem.children[list.id]);
              }
            } else {
              if (sourceTable[elem.id] == undefined) {
                sourceTable[elem.id] = res.push({
                  id: elem.id,
                  label: elem.label,
                  parentId: elem.parentId,
                });
              }
            }
          });
        });
      });
      return res;
    },
  },
  methods: {
    setParentIdForStructures() {
      const data = this.apiData.structures.slice();
      function recursive(data, parentId) {
        data.forEach((item) => {
          item.parentId = parentId;
          if (item.children && item.children.length > 0) {
            return recursive(item.children, item.id);
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

    filterDataWithLevels(data, path = [], initial = []) {
      const res = [...path];
      data.forEach((element) => {
        if (element.lists) {
          element.lists.forEach((listId) => {
            if (!res[res.length - 1].children[listId]) {
              res[res.length - 1].children[listId] = [
                {
                  id: element.id,
                  label: element.label,
                  parentId: element.parentId,
                },
              ];
            } else {
              res[res.length - 1].children[listId].push({
                id: element.id,
                label: element.label,
                parentId: element.parentId,
              });
            }
          });
        } else if (element.children) {
          const newPath = [
            ...path,
            {
              id: element.id,
              label: element.label,
              parentId: element.parentId,
              children: {},
            },
          ];
          initial = this.filterDataWithLevels(
            element.children,
            newPath,
            initial
          );
        }
      });
      if (res.length > 1) {
        initial.push(res);
      }
      return initial;
    },
  },
};
</script>

<style scoped></style>
