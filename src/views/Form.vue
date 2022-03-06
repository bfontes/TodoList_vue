<template>
  <div class= "container mt-2">
    <b-form>
      <b-form-group
        label="Titulo"
        label-for="subject"
      >
        <b-form-input
          id="subject"
          v-model="form.subject"
          type="text"
          placeholder="Ex: Remédio"
          required
          autocomplete="off"
        ></b-form-input>
      </b-form-group>
    
      <b-form-group
        label="Descrição"
        label-for="description"
      >
        <b-form-textarea
          id="description"
          v-model="form.description"
          type="text"
          placeholder="Ex: Tomar remédio às 15hrs da tarde"
          required
          autocomplete="off"
        ></b-form-textarea>
      </b-form-group>

      <b-button type="submit" variant="outline-primary" @click="saveTask"> Salvar </b-button>
    </b-form> 
  </div>
</template>

<script>
import ToastMixin from "@/mixins/toastMixin.js";
export default {
  name: "Form",

  mixins: [ToastMixin],
  
  data() {
    return {
      form: {
        subject: "",
        description: ""
      },
      methodSave: "new"
    }
  },

  created() {
    if(this.$route.params.index === 0 || this.$route.params.index !== undefined){
      this.methodSave = "update";
      let tasks = JSON.parse(localStorage.getItem("tasks"));
      //chave pra salvar os dados 
      this.form = tasks[this.$route.params.index];
    }
  },
  methods: {
    saveTask(){
      if(this.methodSave === "update"){
        let tasks = JSON.parse(localStorage.getItem("tasks"));
        //chave pra salvar os dados atualizados apos edição
        tasks[this.$route.params.index] = this.form;
        localStorage.setItem("tasks", JSON.stringify(tasks));
        this.showToast("success", "Sucesso", "Tarefa atualizada");
        //redirecionar pra tela de listagem
        this.$router.push({ name: "list"});
        return;
      }
      //Pegar todas as tarefas do localStorage e trata-los como objeto no lugar de string
      // Caso nao ache nada, retornar array vazio que vai significar que é a primeira tarefa do array
      let tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : [] ;
      //Salvar o que ta no titulo e descrição dentro das tarefas
      tasks.push(this.form);
      //Salvar as infos no array
      localStorage.setItem("tasks", JSON.stringify(tasks));
      this.showToast("success", "Sucesso", "Tarefa criada");
      //Ir pra rota de lista'
      this.$router.push({ name: "list"});
    }
  }
}
</script>

