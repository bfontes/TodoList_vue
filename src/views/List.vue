<template>
  <div class= "container mt-2">
    <div v-for="(task, index) in tasks" :key="index">
    <b-card :title="task.subject" class="mb-2">
      <b-card-text class="mb-2">{{ task.description }}</b-card-text>
      <b-button variant="outline-secondary" class="mr-2" @click="edit(index)"> Editar</b-button>
      <b-button variant="outline-danger" class="mr-2" @click="remove(task, index)"> Excluir </b-button>
    </b-card>
    </div>

    <b-modal ref="modalRemove" hide-footer title="Excluir tarefa">
       <div class="d-block text-center">
         Deseja realmente excluir a tarefa? {{ taskSelected.subject }}
      </div>
      <div class="mt-3 d-flex jusrify-content-end">
        <b-button variant="outline-secondary" class="mr-2" @click="hideModal"> Cancelar </b-button>
        <b-button variant="outline-danger" class="mr-2" @click="confirmRemoveTask"> Excluir </b-button>
      </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "List",
  
  data() {
    return {
      tasks: [],
      taskSelected: []
    }
  },
  // assim que o componente for criado, executar tarefas
  created() {
    this.tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : [] ;
  },

  methods: {
    edit(index) {
      this.$router.push({ name: "form", params: {index} });
    },
    remove(task, index) {
      this.taskSelected = task;
      this.taskSelected.index = index;
      this.$refs.modalRemove.show()
    },
    hideModal() {
      this.$refs.modalRemove.hide();
    },
    confirmRemoveTask() {
      this.tasks.splice(this.taskSelected.index, 1);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
      this.hideModal;
    }

  }
}
</script>