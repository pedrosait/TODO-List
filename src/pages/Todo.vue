<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input   
        @keyup.enter="addTask"
        class="col"
        square
        filled 
        bg-color="white"
        dense 
        v-model="newTask"
        placeholder="Add task"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input> 
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item tag="label" 
        v-ripple 
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.donde = !task.done"
        clickable
        :class="{ 'done bg-blue-1': task.done}"
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" class="no-pointer-events"/>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section 
          v-if="task.done"
          side
        >
        <q-btn 
          @click.stop="deleteTask(index)"
          flat 
          round 
          color="primary" 
          icon="delete" 
          dense />
        </q-item-section>
      </q-item>
    </q-list>

    <div 
      v-if="!tasks.length"
      class="no-tasks absolute-center">
      <div class="text-h5 text-primary text-center">
        <q-icon 
          name="check"
          size="100px"
          color="primary"
        />
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'TodoItems',
  data() {
    return {
      newTask: '',
      tasks: [
        // {
        //   title:'Comprar un procesador nuevo',
        //   done: false
        // },
        // {
        //   title:'Pagar Spotify',
        //   done: false
        // },
        // {
        //   title:'Hacer cambio de aceite',
        //   done: false
        // },
      ]
    }
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify({
          message: 'Task deleted!',
          color: 'green'
        });
      })
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.$q.notify({
          message: 'Task added!',
          color: 'primary'
        });
      this.newTask = ''
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
  .no-tasks {
    opacity: 0.5;
  }
</style>