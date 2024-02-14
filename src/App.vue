<template>
  <div class="container">
    <div class="listMain">
      <div class="task" v-for="(item, index) in task" :key="index + 'task'">
        <div class="topTask" :style="{ background: item.color }">
          <span>{{ item.name }}</span>
        </div>
        <div class="draggableMain">
          <draggable
            @change="(evt) => {logTaskMove(evt, item);}"
            class="list-group"
            :list="item.data"
            group="task"
            itemKey="name"
          >
            <template #item="{ element, index }">
              <div class="taskItem" v-if="!element.edit">
                <div class="taskName">
                  <span>{{ element.name }}</span>
                </div>
                <img
                  src="./assets/moreHorizontal.svg"
                  @click="element.showAction = true"
                  alt=""
                />

                <div class="action" v-show="element.showAction">
                  <div class="actionItem" @click="editTask(element)">
                    <img src="./assets/edit.svg" alt="" />
                    <span class="actionText">Редактировать</span>
                  </div>
                  <div class="actionItem" @click="remove(item, index)">
                    <img src="./assets/trash.svg" alt="" />
                    <span class="actionText">Удалить</span>
                  </div>
                  <div @click="element.showAction = false" class="actionBg"></div>
                </div>
              </div>
              <div v-else>
                <div class="showAdd">
                  <textarea
                    v-model="element.editing"
                    class="textArea"
                    rows="4"
                  ></textarea>
                  <div class="btnsTextArea">
                    <img
                      @click="saveEdit(element)"
                      class="btns"
                      src="./assets/add.svg"
                      alt=""
                    />
                  </div>
                </div>
              </div>
            </template>
            <template #footer>
              <div v-if="item.showAdd" class="showAdd">
                <textarea
                  v-model="item.text"
                  placeholder="Введите текст..."
                  class="textArea"
                  rows="4"
                ></textarea>
                <div class="btnsTextArea">
                  <img
                    @click="close(item)"
                    class="btns"
                    src="./assets/close.svg"
                    alt=""
                  />
                  <img @click="add(item)" class="btns" src="./assets/add.svg" alt="" />
                </div>
              </div>
              <span v-else class="addTask" @click="item.showAdd = true">+ Добавить</span>
            </template>
          </draggable>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: "App",
  components: {
    draggable,
  },
  data() {
    return {
      task: {
        toBeAgreed: {
          name: "На согласовании",
          data: [
            {
              name: "asdddddd dddddadsas dasdsads adsa",
              showAction: false,
              edit: false,
              editing: "",
            },
          ],
          color: "rgba(255, 153, 233, 1)",
          text: "",
          showAdd: false,
        },
        new: {
          name: "Новые",
          data: [
            {
              name: "asddddddddddadsadasdsadadsa",
              showAction: false,
              edit: false,
              editing: "",
            },
          ],
          color: "rgba(102, 184, 255, 1)",
          text: "",
          showAdd: false,
        },
        inProgress: {
          name: "В процессе",
          data: [],
          color: "rgba(255, 212, 102, 1)",
          text: "",
          showAdd: false,
        },
        ready: {
          name: "Готово",
          data: [],
          color: "rgba(83, 198, 102, 1)",
          text: "",
          showAdd: false,
        },
        finalize: {
          name: "Доработать",
          data: [],
          color: "rgba(247, 110, 133, 1)",
          text: "",
          showAdd: false,
        },
      },
    };
  },
  methods: {
    add(item) {
      if (item.text != "") {
        item.data.push({
          name: item.text,
          showAction: false,
          edit: false,
          editing: "",
        });
        item.text = "";
        item.showAdd = false;
        alert(`Задача создана в ${item.name}`);
      }
    },
    close(item) {
      item.text = "";
      item.showAdd = false;
    },
    editTask(el) {
      el.editing = el.name;
      el.edit = true;
      el.showAction = false;
    },
    saveEdit(el) {
      if (el.editing != "") {
        el.edit = false;
        el.name = el.editing;
      } else {
        alert("Поле не может быть пустым");
      }
    },
    remove(item, index) {
      item.data = item.data.filter((filterItem, filterIndex) => {
        return filterIndex != index;
      });
      alert("Задача удалена");
    },
    logTaskMove(evt, item) {
      if (evt.added) {
        alert(`Задача перемещена в ${item.name}`);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  max-width: 1024px;
  margin: auto;
  font-family: "Interphases";
}
.actionText {
  margin-left: 8px;
  font-size: 14px;
}
.actionItem {
  display: flex;
  align-items: center;
  height: 30px;
  padding: 0px 16px;
  z-index: 100;
  &:hover {
    background: rgba(225, 241, 255, 1);
  }
}
.actionBg {
  position: fixed;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 0px;
  z-index: 99;
}
.action {
  display: flex;
  flex-direction: column;
  padding: 8px 0px 8px 0px;
  border-radius: 4px;
  border: 1px solid rgba(227, 229, 232, 1);
  background: rgba(255, 255, 255, 1);
  position: absolute;
  right: -128px;
  top: 30px;
  z-index: 100;
}
.taskItem {
  padding: 8px 0px 8px 8px;
  border: 1px solid rgba(196, 202, 212, 1);
  border-radius: 4px;
  margin-bottom: 8px;
  background: rgba(255, 255, 255, 1);
  display: flex;
  align-items: flex-start;
  position: relative;

  .taskName {
    width: 80%;
    margin-right: 8px;
    text-overflow: clip;
    overflow-wrap: break-word;
    font-size: 14px;
  }
}
.btns {
  &:hover {
    cursor: pointer;
  }
}
.showAdd {
  padding: 8px;
  border: 1px solid rgba(61, 134, 244, 1);
  border-radius: 4px;
  display: flex;
  background: rgba(255, 255, 255, 1);
  .btnsTextArea {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-left: 4px;
  }
  .textArea {
    width: 100%;
    border: none !important;
    outline: none !important;
  }
}
.addTask {
  color: rgba(61, 134, 244, 1);
  font-size: 14px;
  text-align: center;
  &:hover {
    cursor: pointer;
  }
}
.task {
  min-width: 200px;
  max-width: 200px;
  height: 100%;
  margin-top: 20px;
  margin-right: 12px;
  background: rgba(247, 247, 247, 1);
  border-radius: 0px 0px 12px 12px;

  .topTask {
    border-radius: 12px 12px 0px 0px;
    height: 32px;
    background: rgba(255, 153, 233, 1);
    display: flex;
    align-items: center;
    justify-content: center;
    color: rgba(28, 37, 48, 1);
    font-weight: 600;
    font-size: 14px;
  }

  .draggableMain {
    // overflow-x: hidden;
    // overflow-y: auto;
    padding: 12px;
    height: 100%;
  }
}
.listMain {
  display: flex;
  justify-content: space-between;
  height: 90vh;
  max-width: 920px;
}
</style>
