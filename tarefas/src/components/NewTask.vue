<template>
	<div id="new-task">
        <!--USANDO V-MODEL COMO TWO WAY DATABADING-->
        <!--USANDO MODIFICADOR DE EVENTO @KEYDOWN.ENTER="ADD"-->
        <input v-model="name" @keydown.enter="add" type="text" class="form-element" placeholder="Nova tarefa?">
        <button @click="add" class="form-element">+</button>
	</div>
</template>

<script>
export default {
    //COMPORTAMENTOS DO COMPONENTE
    data(){
        return{
            //ÚNICO CAMPO
            name: ''
        }
    },//METODOS ASSOCIADO AO COMPONENTE
    methods:{
        //EMININTO UM EVENTO COM $emit dentro do componente para o pai INCLUIR UM NOVO NAME JÁ QUE ELE QUE É O DONO DOS DADOS
        add(){
            //EMITINDO UM EVENTO CHAMADO TASKADDED, ADICIONANDO UM NOVO OBJETO DENTRO DO ARRAY TASKS COM NOME DE NAME
            this.$emit('taskAdded', {name: this.name})//ESSE OBJETO SERÁ PASSADO POR PADRÃO, E IRA SER O PARAMETRO task dentro de addTask no pai
            this.name = ''
        }
    }
}
</script>

<style>
    .new-task{
        margin: 35px;
    }
    .form-element{
        outline: none;
        font-size: 2rem;
        border: 1px solid #fff;
        padding: 5px 10px 8px;
        color: #fff;
    }
    input.form-element{
        width:500px;
        background-color: #fff2;
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
    }
    button.form-element{
        cursor: pointer;
        border-left: none;
        background-color: #2196f3;
        border-top-right-radius: 8px;
        border-bottom-right-radius: 8px;
    }
</style>
