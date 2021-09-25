<template>
  <q-page padding>
    <div class="row q-mb-lg">
      <img
        alt="Quasar logo"
        src="~assets/quasar-logo-vertical.svg"
        style="width: 200px; height: 200px"
        class="col"
      >
    </div>
    <div class="row justify-center q-mb-lg">
      <q-input standout="bg-teal text-white" v-model="text" label="Add your task"  class="col-md-4 col-8" @keyup.enter="addTask"/>
      <q-btn color="primary" label="Add Task" @click="addTask"/>
    </div>

    <div class="row justify-center q-mb-lg">
      <q-list bordered class="col-md-6 col-12">
      <q-item-label header>List Task</q-item-label>
      <q-separator />
      <q-item  class="q-my-sm" clickable v-ripple v-for="(task,index) in tasks" :key="task">
        <q-item-section>
          <q-item-label>{{task}}</q-item-label>
        </q-item-section>

        <q-item-section side>
          <q-icon name="check_circle_outline" color="green" @click="doneTask(index)" v-ripple/>
        </q-item-section>
      </q-item>
      </q-list>
    </div>
    <div class="row justify-center">
      <q-list bordered class="col-md-6 col-12">
      <q-item-label header>Done Task</q-item-label>
      <q-separator />
      <q-item  class="q-my-sm" clickable v-ripple v-for="(task,index) in done" :key="task">
        <q-item-section>
          <q-item-label>{{task}}</q-item-label>
        </q-item-section>

        <q-item-section side>
          <q-icon name="close" color="red" @click="deleteTask(index)" v-ripple/>
        </q-item-section>
      </q-item>
      </q-list>
    </div>
  </q-page>
</template>


<script>
import { defineComponent } from 'vue';
import { useQuasar } from 'quasar'



export default defineComponent({
  name: 'PageIndex',
  setup(){
      const $q = useQuasar()
      function deleteTask(index) {
      $q.dialog({
        title: 'Confirm',
        message: 'Really Delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.push(this.done[index])
        this.done.splice(index,1)
        $q.notify({
          message:'Task Deleted',
          color:'red'
        })
        // console.log('>>>> OK')
      }).onOk(() => {
        // console.log('>>>> second OK catcher')
      }).onCancel(() => {
        // console.log('>>>> Cancel')
      }).onDismiss(() => {
        // console.log('I am triggered on both OK and Cancel')
      })
      }
      return {deleteTask}
  },
  data(){
    return {
      tasks:['task1','task2','task3'],
      done:[],
      text:''
    }
  },
  methods: {
    addTask(){
      this.tasks.push(this.text)
      this.text = ''
    },
    doneTask(index){
      this.done.push(this.tasks[index])
      this.tasks.splice(index,1)
    },
  },
})
</script>
