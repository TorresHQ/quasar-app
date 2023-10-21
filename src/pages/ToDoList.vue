<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addNewTask()"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="New task"
        dense
      >
        <template v-slot:append>
          <q-btn
            @click="addNewTask()"
            round
            dense
            flat
            icon="add"
          />
        </template>
      </q-input>

    </div>
    <q-list
      v-if="tasks.length"
      class="bg-white"
      separator
      bordered
    >
       <q-item
        v-for="task in tasks"
        :key="task.title"
        :class="{ 'done bg-yellow-2' : task.done }"
        @click="task.done = !task.done"
        clickable
        v-ripple
       >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side
        >
          <q-btn
            @click.stop="confirm = true; taskIndex = tasks.indexOf(task)"
            flat
            round
            dense
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div
      v-else
      class="no-tasks absolute-center"
    >
      <q-icon
          name="check"
          size="100px"
          color="primary"
        />
      <div class="text-h5 text-primary text-center">
        No Tasks
      </div>
    </div>

    <q-dialog
      v-model="confirm"
      persistent
    >
      <q-card>
        <q-card-section>
          <h6 class="q-mt-none q-mb-sm">Are you sure?</h6>
          <span> Do you really want to delete this task?</span>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn
            flat
            label="No, I don´t"
            color="primary"
            v-close-popup
          />
          <q-btn
            flat
            label="Yes, I do"
            color="primary"
            v-close-popup
            @click="deleteTask()"
          />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar'
import { ref } from 'vue'

const $q = useQuasar()

const tasks = ref([
  { title: 'To feed', done: false },
  { title: 'To clean the litter box', done: false },
  { title: 'To play', done: false },
  { title: 'To brush the fur', done: false },
  { title: 'To buy a new toy', done: false}
])

const taskIndex = ref(null)
const confirm = ref(false)
const newTask = ref('')

const deleteTask = () => {
  tasks.value.splice(taskIndex.value, 1)
  showNotification('Task deleted successfully', 'secondary', 'delete')
}

const showNotification = (message, color, icon) => {
  $q.notify({
    message: message,
    color: color,
    icon: icon,
    position: 'top-right',
  })
}

const addNewTask = () => {
  newTask.value = newTask.value.trim()

  if (newTask.value) {
    tasks.value.push({
      title: newTask.value,
      done: false
    })
    newTask.value = ''
    showNotification('Task added successfully', 'secondary', 'add')
  } else {
    showNotification('First, add a task', 'negative', 'warning')
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
