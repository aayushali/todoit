<template>
  <q-page class="q-pa-md">
    <q-list bordered separator>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.id"
        clickable
        @click="checkCompleted(index)"
        v-ripple
        :class="task.completed ? 'bg-green-2' : 'bg-orange-1'"
      >
        <q-item-section side top>
          <q-checkbox v-model="task.completed" class="no-pointer-events" />
        </q-item-section>
        <q-item-section>
          <q-item-label :class="{ 'text-strike': task.completed }">
            {{ task.name }}
          </q-item-label>
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
        <q-item-section side>
          <q-btn
            flat
            round
            color="red"
            dense
            icon="delete"
            @click.stop="promptToDelete(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-fab icon="add" @click="toggleAddTask" direction="up" color="accent">
      </q-fab>
    </q-page-sticky>
    <q-dialog v-model="showAddTask">
      <q-card>
        <q-form @submit.prevent="handleSubmit">
          <q-card-section class="row">
            <div class="text-h6">Add Task</div>
            <q-space />

            <q-btn dense flat round icon="close" v-close-popup />
          </q-card-section>

          <q-card-section>
            <div class="row q-mb-sm">
              <q-input
                outlined
                v-model="taskToSubmit.name"
                label="Task name"
                class="col"
                :rules="[(val) => !!val || 'Field is required']"
              ></q-input>
            </div>
            <div class="row q-mb-sm">
              <q-input
                label="Due Date"
                class="q-py-sm"
                outlined
                v-model="taskToSubmit.dueDate"
              >
                <template v-slot:append>
                  <q-icon name="event" class="cursor-pointer">
                    <q-popup-proxy
                      ref="qDateProxy"
                      transition-show="scale"
                      transition-hide="scale"
                    >
                      <q-date v-model="taskToSubmit.dueDate">
                        <div class="row items-center justify-end">
                          <q-btn
                            v-close-popup
                            label="Close"
                            color="primary"
                            flat
                          />
                        </div>
                      </q-date>
                    </q-popup-proxy>
                  </q-icon>
                </template>
              </q-input>
            </div>
            <div class="row q-mb-sm">
              <q-input label="Due Time" outlined v-model="taskToSubmit.dueTime">
                <template v-slot:append>
                  <q-icon name="access_time" class="cursor-pointer">
                    <q-popup-proxy
                      transition-show="scale"
                      transition-hide="scale"
                    >
                      <q-time v-model="taskToSubmit.dueTime">
                        <div class="row items-center justify-end">
                          <q-btn
                            v-close-popup
                            label="Close"
                            color="primary"
                            flat
                          />
                        </div>
                      </q-time>
                    </q-popup-proxy>
                  </q-icon>
                </template>
              </q-input>
            </div>
          </q-card-section>

          <q-card-actions align="right">
            <q-btn flat label="Save" color="primary" type="submit" />
          </q-card-actions>
        </q-form>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      showAddTask: false,
      taskToSubmit: {
        id: new Date().toLocaleString(),
        name: "",
        dueDate: "2019/04/30",
        dueTime: "",
        completed: false,
      },
      tasks: [
        {
          id: 1,
          name: "Buy Milk and Coffee",
          completed: false,
          dueDate: "2019/05/12",
          dueTime: "18:30",
        },
        {
          id: 2,
          completed: false,
          name: "Complete Assignment",
          dueDate: "2021/05/12",
          dueTime: "13:15",
        },
        {
          id: 3,
          completed: false,
          name: "Washing Dishes",
          dueDate: "2021/05/13",
          dueTime: "17:50",
        },
        {
          id: 4,
          completed: false,
          name: "Pay the electricity bills.",
          dueDate: "2021/05/14",
          dueTime: "20:30",
        },
        {
          id: 5,
          completed: false,
          name: "Update the project and list tasks",
          dueDate: "2021/07/12",
          dueTime: "21:40",
        },
      ],
    };
  },

  methods: {
    promptToDelete(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Do you want to Delete?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          console.log(">>>> OK");
          this.tasks.splice(index, 1);
        })
        .onCancel(() => {});
    },
    checkCompleted(index) {
      console.log("completed");
      this.tasks[index].completed = !this.tasks[index].completed;
    },

    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    handleSubmit() {
      console.log("submit form", this.taskToSubmit);
      this.tasks.push(this.taskToSubmit);
      console.log(this.tasks);
    },
  },
};
</script>
 

