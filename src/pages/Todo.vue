<template>
  <q-page class="column q-pa-lg bg-grey-3">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask()"
        class="col bg-white"
        square
        filled
        placeholder="Add Task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask()" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list 
      class="bg-white" 
      separator 
      bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(task, index)"
            flat
            dense
            round
            icon="delete"
            color="primary"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div 
      v-if="!tasks.length"
      class="absolute-center text-h5"
      style="opacity:0.3">
        <div>
          <q-icon name="check" size="100px" />
        </div>
        <span>No Tasks</span>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },
  methods: {
    deleteTask(task, index) {
      this.$q
        .dialog({
          title: "Confirm Deletion of Task",
          message: `${task.title}`,
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task deleted");
        });
    },
    addTask() {
      if (this.newTask !== '') {
        this.tasks.push({
          title: this.newTask,
          done: false
        })
        this.newTask = ''
      }
    }
  }
}
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
