<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-white">
      <q-input
        bg-color="white"
        v-model="task"
        placeholder="Add Task"
        square
        filled
        dense
        class="col"
        @keyup.enter="addTask"
      >
        <template v-slot:before>
          <q-icon name="add_task" />
        </template>
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>

    <q-list class="bg-white" separator bordered>
      <q-item
        v-ripple
        v-for="(task, index) in tasks"
        :key="index"
        :class="{ 'done bg-green-2': task.done }"
        @click="task.done = !task.done"
        clickable
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="positive" />
        </q-item-section>

        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section side v-if="task.done">
          <q-btn
            color="red-5"
            flat
            dense
            round
            icon="delete"
            @click.stop="deleteTask(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <div class="no-tasks absolute-center" v-if="!tasks.length">
      <q-icon name="check" size="100px" color="positive" />

      <div class="text-h5 text-center">No Tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "Todo",
  data() {
    return {
      tasks: [],
      task: ""
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Do you really want to delete it?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task deleted");
        });
    },
    addTask() {
      if (this.task == "") {
        return;
      }

      this.tasks.push({
        title: this.task,
        done: false
      });
      this.task = "";
      this.$q.notify("Task added");
    }
  }
};
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
