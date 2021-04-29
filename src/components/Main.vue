<template>
  <div class="main">
    <input type="checkbox" name="Levels" v-model="withLevels" />
    <label for="Levels">Режим подразделений</label>
    <div v-for="(item, id) in filteredData" :key="id">
      <p :class="{ list: item.list }">{{ item }}</p>
    </div>
  </div>
</template>

<script>
import data from "@/new.json";
export default {
  data() {
    return {
      apiData: data,
      withLevels: false,
    };
  },
  computed: {
    filteredData() {
      const structures = this.setParentIdForStructures(this.apiData.structures);
      const listsObject = {}; // сделаем объект из lists, чтобы ускорить поиск т.к. искать в них будем часто и список может быть большим

      this.apiData.lists.forEach((item) => {
        listsObject[item.id] = item;
      });

      if (this.withLevels) {
        return this.parseFiltredDataWithLevels(
          this.filterDataWithLevels({
            data: structures,
            path: [],
            listsObject: listsObject,
            initial: [],
          })
        );
      } else {
        const filterDataResult = this.filterData({
          data: structures,
          listsObject: listsObject,
          parentId: null,
          container: {},
        });
        const res = [{ label: "Выбрать всех" }];
        this.apiData.lists.forEach((list) => {
          res.push(...filterDataResult[list.id]);
        });
        return res;
      }
    },
  },
  methods: {
    setParentIdForStructures(structures) {
      const data = structures.slice();
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
    filterData(payload) {
      payload.data.forEach((element) => {
        if (element.lists) {
          // Полагаю, что поле "lists" есть только у пользователей и у пользователей нет других "children"
          element.lists.forEach((listId) => {
            if (payload.listsObject[listId]) {
              if (!payload.container[listId]) {
                payload.container[listId] = [
                  Object.assign({ list: true }, payload.listsObject[listId]), // list: true - для выделения строчки в разметке и назнечения CSS класса
                ];
              }
              payload.container[listId].push(
                Object.assign({}, element, {
                  parentId: payload.parentId,
                  currentListId: listId,
                }) // Добавляем копию т.к. иначе добавление пользователя в другой список перезапишет поле currentListId у всех экземпляров
              );
            }
          });
        } else if (element.children) {
          this.filterData({
            data: element.children,
            listsObject: payload.listsObject,
            parentId: element.id,
            container: payload.container,
          });
        } else {
          return;
        }
      });
      return payload.container;
    },
    filterDataWithLevels(payload) {
      const res = [...payload.path];
      payload.data.forEach((element) => {
        if (element.lists) {
          element.lists.forEach((listId) => {
            if (payload.listsObject[listId]) {
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
            }
          });
        } else if (element.children) {
          const newPath = [
            ...payload.path,
            {
              id: element.id,
              label: element.label,
              parentId: element.parentId,
              children: {},
            },
          ];
          payload.initial = this.filterDataWithLevels({
            data: element.children,
            path: newPath,
            listsObject: payload.listsObject,
            initial: payload.initial,
          });
        }
      });
      if (res.length > 1) {
        payload.initial.push(res);
      }
      return payload.initial;
    },
    parseFiltredDataWithLevels(filterDataWithLevels) {
      const res = [{ label: "Выбрать всех" }];
      this.apiData.lists.forEach((list) => {
        res.push(Object.assign({ list: true }, list)); // list: true - для выделения строчки в разметке и назнечения CSS класса
        const sourceTable = {};
        filterDataWithLevels.forEach((item) => {
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
};
</script>

<style scoped>
.list {
  background-color: cornflowerblue;
}
</style>
