<template>
  <div>
    <h1>Kanvan Board</h1>
    <b-input-group class="task-input-container">
      <b-form-input
        v-model="newTask"
        placeholder="업무를 입력하세요"
        v-on:keyup.enter="addTask"
      ></b-form-input>
      <b-input-group-append>
        <b-button v-on:click="addTask" variant="primary">추가</b-button>
      </b-input-group-append>
    </b-input-group>
    <b-card-group deck class="board-container">
      <b-card
        bg-variant="light"
        text-variant="black"
        header="할 일"
        v-on:drop="drop($event, 'arrBackLog')"
        v-on:dragover="allowDrop"
      >
        <b-list-group>
          <b-list-group-item
            class="work-list-item"
            v-for="(item, index) in arrBackLog"
            v-bind:key="index"
            draggable="true"
            v-on:dragstart="drag(item.name, index, 'arrBackLog')"
          >
            {{ item.name }}
          </b-list-group-item>
        </b-list-group>
      </b-card>
      <b-card
        bg-variant="warning"
        text-variant="white"
        header="진행중"
        v-on:drop="drop($event, 'arrInProgress')"
        v-on:dragover="allowDrop"
      >
        <b-list-group>
          <b-list-group-item
            class="work-list-item"
            v-for="(item, index) in arrInProgress"
            v-bind:key="index"
            draggable="true"
            v-on:dragstart="drag(item.name, index, 'arrInProgress')"
          >
            {{ item.name }}
          </b-list-group-item>
        </b-list-group>
      </b-card>
      <b-card
        bg-variant="success"
        text-variant="white"
        header="끝난 일"
        v-on:drop="drop($event, 'arrDone')"
        v-on:dragover="allowDrop"
      >
        <b-list-group>
          <b-list-group-item
            class="work-list-item"
            v-for="(item, index) in arrDone"
            v-bind:key="index"
            draggable="true"
            v-on:dragstart="drag(item.name, index, 'arrDone')"
          >
            {{ item.name }}
          </b-list-group-item>
        </b-list-group>
      </b-card>
    </b-card-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      arrBackLog: [
        {
          name: "세금계산서 수령",
        },
        {
          name: "야유회 일정 공지",
        },
      ],
      arrInProgress: [],
      arrDone: [],
      dragItem: "",
      dragIdx: -1,
      fromArrayName: "",
      toArrayName: "",
    };
  },
  methods: {
    addTask() {
      this.arrBackLog.push({ name: this.newTask });
      this.newTask = "";
    },
    drag(item, idx, fromArrayName) {
      this.dragItem = item;
      this.dragIdx = idx;
      this.fromArrayName = fromArrayName;
    },
    allowDrop(ev) {
      ev.preventDefault();
    },
    drop(ev, toArrayName) {
      ev.preventDefault();
      this.toArrayName = toArrayName;
      if (this.fromArrayName === this.toArrayName) {
        return;
      }
      // push dragItem
      if (this.toArrayName === "arrBackLog") {
        this.arrBackLog.push({ name: this.dragItem });
      } else if (this.toArrayName === "arrInProgress") {
        this.arrInProgress.push({ name: this.dragItem });
      } else {
        this.arrDone.push({ name: this.dragItem });
      }
      // delete el fromArray
      if (this.fromArrayName === "arrBackLog") {
        this.arrBackLog.splice(this.dragIdx, 1);
      } else if (this.fromArrayName === "arrInProgress") {
        this.arrInProgress.splice(this.dragIdx, 1);
      } else {
        this.arrDone.splice(this.dragIdx, 1);
      }
    },
  },
};
</script>

<style scoped>
.task-input-container,
.board-container {
  margin-left: 20px;
  margin-right: 20px;
}
.task-input-container {
  margin-bottom: 30px;
  width: 300px;
}
.work-list-item {
  color: black;
}
.work-list-item:hover {
  background-color: #dee2e6;
}
</style>