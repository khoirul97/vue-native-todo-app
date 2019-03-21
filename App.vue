<template>
  <view class="container">
    <text class="text-color-primary">My Vue Native App...!</text>
    <text-input
      :style="{fontSize:14, paddingTop:10, paddingBottom:10, paddingLeft:5, marginBottom:10}"
      placeholder="Enter task name.."
      v-model.trim="textContent"
    />
    <view class="btn-wrapper">
      <touchable-opacity class="add-todo-btn" v-bind:onPress="addNewTask">
        <text class="btn-text-color">Add</text>
      </touchable-opacity>
    </view>
    <scroll-view :content-container-style="{
        paddingVertical: 20,
      }">
      <view v-for="(task,index) in taskList" :key="task.id" class="list-item">
        <view :style="{flex:1}" v-if="!task.editable">
          <touchable-opacity :onPress="() => changeTaskStatus(index)">
            <text
              class="todo-text"
              :class="{'completed':task.isCompleted}"
            >{{index+1}}. {{task.name}}</text>
          </touchable-opacity>
        </view>
        <view :style="{flex:1}" v-if="task.editable">
          <text-input
            :style="{fontSize:13, padding:5}"
            placeholder="Enter task name.."
            v-model.trim="task.name"
          />
        </view>
        <view :style="{ flexDirection: 'row'}">
          <touchable-opacity
            class="button edit-btn"
            :onPress="() => editPress(index)"
            v-if="!task.editable && !task.isCompleted"
          >
            <text class="btn-text">Edit</text>
          </touchable-opacity>
          <touchable-opacity
            class="button done-btn"
            :onPress="() => updateTaskName(index)"
            v-if="task.editable"
          >
            <text class="btn-text">Done</text>
          </touchable-opacity>
          <touchable-opacity
            class="button delete-btn"
            :onPress="() => deletePress(index)"
            v-if="!task.editable"
          >
            <text class="btn-text">Delete</text>
          </touchable-opacity>
        </view>
      </view>
    </scroll-view>
  </view>
</template>
 <script>
import { StyleSheet } from "react-native";
export default {
  data: function() {
    return {
      textContent: "",
      btnTitle: "Submit",
      taskList: []
    };
  },
  methods: {
    addNewTask() {
      if (this.textContent.trim().length) {
        this.taskList.push({
          name: this.textContent,
          id: new Date().getTime(),
          editable: false,
          isCompleted: false
        });
        this.textContent = "";
      } else {
        alert("Please enter the task name to add.");
      }
    },
    deletePress(index) {
      // alert('delete this item'+index);
      this.taskList.splice(index, 1);
    },
    editPress(index) {
      this.taskList[index].editable = true;
    },
    updateTaskName(index) {
      this.taskList[index].editable = false;
    },
    changeTaskStatus(index) {
      this.taskList[index].isCompleted = !this.taskList[index].isCompleted;
    }
  },
  mounted() {
    const styles = StyleSheet.create({
      bigblue: {
        color: "blue",
        fontWeight: "bold",
        fontSize: 30
      },
      red: {
        color: "red"
      }
    });
  }
};
</script>
<style>
.btn-wrapper {
  justify-content: center;
  align-items: flex-end;
}
.container {
  /* background-color: white; */
  padding: 20;
  /* paddingTop: 40; */
  /* align-items: center; */
  /* justify-content: center; */
  /* flex: 1; */
}
.btn-text-color {
  color: #fff;
}
.text-input {
  width: 300;
  font-size: 14;
  padding: 10;
}
.add-todo-btn {
  width: 60;
  align-items: center;
  justify-content: center;
  background-color: rgb(63, 104, 236);
  padding: 5;
  border-radius: 4;
  /* flex-direction: row; */
  /* padding-horizontal: 10; */
}
.button {
  /* background-color: #fff; */
  width: 50;
  align-items: center;
  justify-content: center;
  margin-left: 5;
  padding: 5;
  border-radius: 4;
  /* float:right; */
  /* flex: 1; */
}
.btn-text {
  color: #fff;
  align-items: center;
  justify-content: center;
  /* width: 100; */
}
.todo-text {
  /* width: 80; */
  align-items: flex-start;
  justify-content: center;
}
.list-item {
  /* width: 300; */
  padding: 8;
  border-width: 1;
  border-color: #ddd;
  background-color: #eee;
  margin-bottom: 10;
  /* align-items: flex-start; */
  /* justify-content: center; */
  flex-direction: row;
}
.completed {
  text-decoration-line: line-through;
  font-style: italic;
}
.edit-btn {
  background-color: rgb(63, 104, 236);
}
.delete-btn {
  background-color: rgb(255, 0, 0);
}
.done-btn {
  background-color: rgb(0, 0, 0);
}
</style>