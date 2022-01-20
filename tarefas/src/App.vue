<template>
	<div id="app">
		<h1>Tarefas</h1>
		<!--tasksprogress apontando para propriedade progress-->
		<TasksProgress :progress="progress"/>
		<!--INVOCANDO COMPONENTE NEWTASK-->
		<!--PRECISAMOS TRATAR O EVENTO EMITIDO DE NEWTASK-->
		<!--PRECISAMOS CRIAR UM METHODS PARA addTask-->
		<NewTask @taskAdded="addTask"/>
		<br>
		<!--:tasks apontando para o atributo tasks para que possa interpretar o array tasks e passar o array tasks para o componente TaskGrid e dentro do componente fazer um v-for da lista-->
		<!--PRECISAMOS TRATAR O $EVENT TASKDELETED-->
		<!--@$EMIT RECEBIDOS = "METHODO CRIADO"-->
		<TaskGrid 
		@taskStateChanged="toggleTaskState"
		@taskDeleted="deleteTask"
		 :tasks="tasks"/>
	</div>
</template>

<script>
//IMPORTANDO COMPONENTES
import TaskGrid from './components/TaskGrid.vue'
import NewTask from './components/NewTask.vue'
import TasksProgress from './components/TasksProgress.vue'

export default {
	//COMPONENTES REGISTRADOS
	components:{
		TaskGrid, NewTask, TasksProgress
	},
	data(){
		return{
			tasks:[
				//LISTA DE TAREFAS
				/*{name:'Lavar a louça', pending:false},
				{name:'Comprar blusa', pending:true}*/
			]
		}
	},
	computed:{
		progress(){
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			// ||0 PARA GARANTIR QUE NÃO VOLTE COMO NOT A NUMBER, AI POR DEFAULT USAMOS || 0
			return Math.round(done / total * 100) || 0
		}
	},
	//PRECISAMOS FAZER COM QUE WATCH DETEQUETE AS MUDANÇAS QUE ACONTECE DENTRO DO ARRAY
	watch:{
		//VAI FICAR MONITORANDO AS TASKS SEMPRE QUE O ARRAY MUDAR
		//TASK VIRA UM OBJETO
		tasks:{
			//WATCH PROFUNDO OLHANDO ADD, REMOVE, TROCA DE ORDEM etc....
			deep:true,
			//metodo a ser chamado quando houver uma mudança dentro do array
			handler(){
			//ESTAMOS PEGANDO A LISTA DE TASKS THIS.TASKS, CONVERTENDO PRA UMA STRING E SETANDO NO LOCALSTORAGE APARTIR DA CHAVE 'TASKS'
			localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		}
	},
	methods:{
		//TASK É O PARAMETRO QUE VEM ASSOCIADO COM O EVENTO TASKADDED POR PADRÃO
		addTask(task){
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0
			if(reallyNew){
				this.tasks.push({
					name: task.name,
					pending: task.pending || true
				})
			}
		},
		//AQUI ESTAMOS RECEBENDO I COMO PARAMETRO PORQUE VAMOS DELETAR POR INDICE 
		deleteTask(i){
			//A DELEÇÃO ESTA SENDO PROPAGADA DE TASK -> TASKGRID -> APP
			this.tasks.splice(i, 1)
		},
		toggleTaskState(i){
			this.tasks[i].pending = !this.tasks[i].pending
		}
	},
	//PELO FATO DO WATCH TASKS QUE ESTÁ SENDO CARREGADO DO LOCALSTORAGE, PRECISAMOS CARREGAR ESSES VALORES NA CRIAÇÃO
	//METHODO CREATED (CYCLO DE VIDA)
	created(){
		//LENDO VALORES DO LOCALSTORAGE EM FORMATO STRING
		const json = localStorage.getItem('tasks')
		//AGORA PODEMOS CONVERTER EM UM ARRAY
		this.tasks = JSON.parse(json) || []
		//VERIFICANDO SE É UM ARRAY
		this.tasks = Array.isArray(array)? array: []
	}
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
