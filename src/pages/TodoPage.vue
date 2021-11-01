<template>
  <q-page class="q-pa-md">
    <q-list bordered separator>
      <q-item
        v-for="task in tasks"
        :key="task.id"
        clickable
        @click="task.completed = !task.completed"
        v-ripple
        :class="task.completed ? 'bg-green-2' : 'bg-orange-1'"
      >
        <q-item-section side top>
          <q-checkbox v-model="task.completed" />
        </q-item-section>
        <q-item-section>
          <q-item-label :class="{ 'text-strikethrough': task.completed }">
            {{ task.name }}</q-item-label
          >
        </q-item-section>
        <q-item-section side>
          <div class="row">
            <div class="column justify-center">
              <q-icon name="event" size="18px" class="q-mr-xs" />
            </div>
            <div class="column">
              <q-item-label caption class="row justify-end">{{
                task.dueDate
              }}</q-item-label>
              <q-item-label caption class="row justify-end">
                <small> {{ task.dueTime }}</small></q-item-label
              >
            </div>
          </div>
        </q-item-section>
      </q-item>
    </q-list>

    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-fab icon="add" direction="up" color="accent">
        <q-fab-action @click="onClick" color="primary" icon="post_add" />
        <q-fab-action @click="onClick" color="primary" icon="settings" />
      </q-fab>
    </q-page-sticky>
  </q-page>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  computed: {
    ...mapGetters("tasks", ["tasks"]),
  },
  methods: {
    deleteTodo(index) {
      this.tasks.splice(index, 1);
    },
    onClick() {
      // console.log('Clicked on a fab action')
    },
  },
};
</script>
