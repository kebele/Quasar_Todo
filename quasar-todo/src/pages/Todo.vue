<template>
  <!-- <q-page class="flex flex-center"> sayfadaki herşeyi center yapıyor -->
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input class="col" square filled bg-color="white" v-model="newTask" @keyup.enter="addTask" label="add task" dense>
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        clickable
        v-ripple
        :class="{ 'done bg-blue-1': task.done }"
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
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="red"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="text-h5 text-primary text-center">
        no tasks
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask : '',
      tasks: [
        // {
        //   title: "ronaldo",
        //   done: false
        // },
        // {
        //   title: "messi",
        //   done: false
        // },
        // {
        //   title: "benzema",
        //   done: false
        // }
      ]
    };
  },
  methods: {
    deleteTask(index) {
      //quasar dan plugins den dialog dan confirm in kodunu alıp koyduk
      this.$q
        .dialog({
          //bunun çalışması için quasar.conf.js de plugin de ['Dialog'] eklemeliyiz, aynı şekilde notify ın çalışması içinde ayunı işlemi yapmalıyız
          title: "Confirm",
          message: "Are you sure?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          //index i alıp 1 tane silecek, index neyse onu silecek
          this.tasks.splice(index, 1);
          this.$q.notify("task deleted!");
        });
    },
    addTask(){
      // console.log('addTask ');
      this.tasks.push({
        title : this.newTask,
        done : false
      })
      this.newTask = ''
    }
  }
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: red;
  }
}
.no-tasks{
  opacity: 0.5;
}
</style>
