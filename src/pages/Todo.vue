<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        bg-color="white"
        class="col"
        filled
        bottom-slots
        v-model="newTask"
        placeholder="Add Task"
        dense
        @keyup.enter="addTask"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        v-ripple
        @click="task.done = !task.done"
        clickable
        :class="{'done bg-blue-1' : task.done}"
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primaty" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" flat dense round color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>

<script lang="ts">
import Vue from "vue";

import ExampleComponent from "components/CompositionComponent.vue";
import { log } from "util";
import {LocalStorage, SessionStorage} from 'quasar';

export default Vue.extend({
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "Get Bananas",
        //   done: false
        // },
        // {
        //   title: "Eat Bananas",
        //   done: false
        // },
        // {
        //   title: "Poo Bananas",
        //   done: false
        // }
      ]
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Delete The Task?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "Task Deleted Successfully!",
            color: "primary"
          });
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      });
      let key=0;
      this.$q.localStorage.set(key,this.tasks);
      this.$q.sessionStorage.set(key,this.tasks);
      key++;
      
      
      this.newTask = "";
    }
  }
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
