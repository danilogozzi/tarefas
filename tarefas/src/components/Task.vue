<template>
    <!--ADD CLASSE COMPUTADA CHAMADA COMO PROPRIEDADE-->
	<div @click="$emit('taskStateChanged', task)"
        class="task"
        :class="stateClass"
    >   <!--$EMIT ESTÁ COM UM NÍVEL A MAIS NA HIERARQUIA-->
        <!--SE HOUVER MUITOS EVENTOS EM MUITAS HIERARQUIAS PODEMOS USAR O $EVENTBUS-->
        <!--NÃO PODEMOS TRATAR TASK EM APP PQ NÃO TEM TASK EM APP, LOGO PRECISAMOS TRATAR O $EMIT DE TASK EM TASKCRID POR CAUSA DO TASK-->
        <!--PRECISAMOS DO .STOP PARA NÃO PROPAGAR MAIS O $EMIT PARA OUTROS $EMIT-->
        <span @click.stop="$emit('taskDeleted', task)" class="close">x</span>
        <!--INTERPOLANDO VALOR DE NAME DE TASK-->
        <p>{{task.name}}</p>
	</div>
</template>

<script>
export default {
    props:{
        task:{
            type:Object, required:true
        }
    },
    //PARA CALCULAR O TIPO DA CLASSE QUE VAMOS APLICAR, PENDENTE OU NÃO
    computed:{
        stateClass(){
            return{
                //CLASSES CSS QUE REPRESENTA O ESTADO DA TAREFA
                //SE A TAREFA ESTIVER PENDENTE
                pending: this.task.pending,
                //SE A TAREFA NÃO ESTIVER PENDENDE
                done: !this.task.pending
            }
        }
    }
}
</script>

<style>
 .task{
     position: relative;
     box-sizing: border-box;
     width: 350px;
     height: 150px;
     padding: 10px;
     border-radius: 8px;
     font-size: 2rem;
     font-weight: 300;
     cursor: pointer;
     user-select: none;
     display: flex;
     justify-content: center;
     align-items: center;
     margin:0.5rem;
 }
 .pending{
     border-left: 12px solid  #b73229;
     background-color: #f44336;
 }
 .done{
     color: #ddd;
     border-left: 12px solid #0a8f08;
     background-color: #4caf50;
     text-decoration: line-through;
 }
 .pending .close{
     background-color: #b73229;
 }
 .done .close{
     background-color: #0a8f08;
 }
 .close{
     position: absolute;
     right: 10px;
     top: 10px;
     font-size: 0.9rem;
     font-weight: 600;
     height: 20px;
     width: 20px;
     border-radius: 10px;
     display: flex;
     justify-content: center;
 }
</style>
