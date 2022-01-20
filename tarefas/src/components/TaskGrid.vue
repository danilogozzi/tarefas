<template>
	<div id="task-grid">
        <!--ESTAMOS USANDO TEMPLATE PQ VAI TER UM V-FOR ASSOCIADO-->
        <!--NÃO VAMOS ACEITAR TAREFAS COM O MESMO NOME, ENTÃO VAMOS USARA KEY COMO NAME-->
		<!--V-IF PARA AVALIAR SE HÁ TAREFA OU NÃO-->
        <template v-if="tasks.length">
            <!--CRIANDO A LISTA DE TAREFAS E INTERPOLANDO O VALOR NAME DA LISTA-->
            <!--task dentro de v-for representa cada task dentro do array tasks-->
            <!--O COMPONENTE TASK ESPERA RECEBER :TASK="TASK" PARA PEGAR CADA TASK QUE VC ESTA VARRENDO PELO V-FOR E PASSA NO PARAMETRO :TASK-->
            <!--PRECISAMOS TRATAR O $EMIT DE TASK QUE É TASKDELETED PARA AI SIM SER PASSADO PARA APP, BUSCANDO PELO INDICE-->
            <Task v-for="(task, i) in tasks"
             :key="task.name" 
             :task="task"
             @taskStateChanged="$emit('taskStateChanged', i)"
             @taskDeleted="$emit('taskDeleted', i)"
             ></Task>
            <!--CADA OBJETO TASK SERA PASSADO COMO PARAMETRO PARA :TASK-->
        </template>
        <p v-else class="no-task">Sua vida está em dia :)</p>
	</div>
</template>

<script>
import Task from './Task.vue'

export default {
    components:{
        Task
    },
    //RECEBENDO LISTA DE TAREFAS
	props:{
        tasks: {type:Array, required:true}
    }
}
</script>

<style>
    #task-grid{
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }
    .task-grid .task{
        margin:10px;
    }
    .no-task{
        color: #aaa;
        font-size: 1.7rem;
    }
</style>
