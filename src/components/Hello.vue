<template>
    <div class="container-fluid content">
      <header>
        <span class="logo">VITTA TO-DO</span>
        <button class="btn btn-criaTarefa" data-toggle="modal" data-target="#myModal"><span class="corCriaTarefa">Criar tarefa </span><i class="glyphicon glyphicon-calendar calendario"></i></button>
      </header>

      <div class="modal fade" id="editModal" role="dialog">
        <div class="modal-dialog">

        <!-- Modal content-->
          <div class="modal-content">
            <div class="modal-header">
              <button type="button" class="close" data-dismiss="modal">&times;</button>
              <h4 class="modal-title">Cadastro de tarefas</h4>
            </div>
            <div class="modal-body">
              <div class="form-group">
                <label for="">Descrição</label>
                <textarea  rows="3" cols="20" class="form-control" id="descricao" v-model="descricao"></textarea>
                <label for="data">Data</label>
                <input type="text" id="data" placeholder="Dia/mês/Ano" class="form-control" v-model="data">
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-success" data-dismiss="modal" v-on:click="addTarefa()">Salvar</button>
              <button type="button" class="btn btn-danger" data-dismiss="modal">Fechar</button>
            </div>
          </div>

      </div>
    </div>
    <section class="main">
      <ul class="list-group" >
        <li class="list-group-item" v-for="item in list" v-bind:class="{ tarefaDeletada: item.desativado }" :key="item.cod">
          <div class="row">
            <div class="col-xs-12">
              <div class="col-xs-1">
                <input type="checkbox" @click="removeTarefa(item.cod)" id="finalizado"></button>
              </div>
              <div class="col-xs-6">
                <label class="divDescricao">{{item.descricao}}</label>
              </div>
              <div class="col-xs-4">
                <span class="spanDataVencimento">Vencimento em:</span><span class="badge dataVencimento">{{item.data}}</span>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </section>

      <div class="modal fade" id="myModal" role="dialog">
        <div class="modal-dialog">

        <!-- Modal content-->
          <div class="modal-content">
            <div class="modal-header">
              <button type="button" class="close" data-dismiss="modal">&times;</button>
              <h4 class="modal-title">Cadastro de tarefas</h4>
            </div>
            <div class="modal-body">
              <div class="form-group">
                <label for="">Descrição</label>
                <textarea  rows="3" cols="20" class="form-control" id="descricao" v-model="descricao"></textarea>
                <label for="data">Data</label>
                <input type="text" id="data" placeholder="Dia/mês/Ano" class="form-control" v-model="data">
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-success" data-dismiss="modal" v-on:click="addTarefa()">Salvar</button>
              <button type="button" class="btn btn-danger" data-dismiss="modal">Fechar</button>
            </div>
          </div>

      </div>
    </div>

    </div>
</template>

<script>
import moment from 'moment-timezone'
export default {
  name: 'tarefas',
  data () {
    return {
      list: []
    }
  },
  created: function() {
    var lista = [];
    if(localStorage.length != 0){
      for( var i = 1;lista.length < localStorage.length; i++){
        var aux = localStorage.getItem(i);
        if(aux != null)
          lista.push(JSON.parse(aux));
      }
      this.list = lista;
    }
  },
  methods:{
    addTarefa() {
      var d = moment.tz('America/Sao_Paulo');
      var nova_d = d.format('DD/MM/YYYY : HH:mm:ss');
      var dataAux = moment(this.data,'DD/MM/YYYY').format('DD/MM/YYYY');
      var cod = 0;
      var arrayTarefa = [];
      var i = 1;
      while(true){
        var objAux = localStorage.getItem(i);
        if(objAux == null){
            cod = i;
            break;
        }else{
            i++;
        }
    }
    var tarefa = {
      cod: cod,
      descricao: this.descricao,
      data: this.data,
      dataUltimaAtualizacao: nova_d,
      dataDel: '',
      desativado: false
    };
    localStorage.setItem(tarefa.cod, JSON.stringify(tarefa));
    this.list.push(tarefa);
    this.descricao = '',
    this.data = ''
  },
  removeTarefa(cod){
    var obj = JSON.parse(localStorage.getItem(cod));
    var listAux = [];
    var d = moment.tz('America/Sao_Paulo');
    var nova_d = d.format('DD/MM/YYYY : HH:mm:ss');
    obj.dataUltimaAtualizacao = nova_d;
    if(!obj.desativado){
      obj.desativado = true;
      obj.dataDel = nova_d;
      obj.dataUltimaAtualizacao = nova_d;
    }
    else{
      obj.dataDel = '';
      obj.desativado = false;
      obj.dataUltimaAtualizacao = nova_d;
    }
    localStorage.setItem(cod.toString(),JSON.stringify(obj));
    var i = 1;
    while(true){
        if(listAux.length >= localStorage.length)
          break;
        var aux = localStorage.getItem(i);
        if(aux != null){
            listAux.push(JSON.parse(aux));
        }
        i++;
    }
    this.list = listAux;
  }
  }
}
</script>

<style scoped>
#descricao{
  resize: none;
}

p{
text-align: justify;
}

ul{
  list-style: none;
}

th, td{
  text-align: center;
}
.tarefaDeletada {
  background-color: #E7FDF6;
}
.dataDel{
  text-decoration: none;
}
.btn-criaTarefa{
  padding: 10px;
  background-color: #27D49E;
  position: relative;
  left: 70%;
}
.calendario{
  margin: 0 auto;
  width: 17px;
  height: 17px;
  color: white;
}
.content{
  margin: 0 auto;
  max-width: 1200px;
  width: 100%;
  padding: 30px;
  padding-top: 0px;
}
.logo{
  font-size: 20px;
  color: white;
}
.corCriaTarefa{
  color: white;
}

.btnSetaEsquerda{
  background-color: white;
  margin-left: 25px;
  margin-top: 20px;
}

.btnSetaDireita{
  background-color: white;
  margin-left: 25px;
  margin-top: 20px;
  position: relative;
  left: 84%;
}
header{
  background-color: #2478AE;
  height: 80px;
  padding: 20px;
}
nav{
  background-color: #EAEBEE;
  height: 80px;
  width: 100%;
}

.glyphicon-menu-left{
  color:#AFAFAF;
}

@media only screen and (max-width: 1024px){
  .logo{
    font-size: 15px;
  }
.btnSetaDireita{
  background-color: white;
  margin-left: 25px;
  margin-top: 20px;
  position: relative;
  left: 80%;
}
}
@media only screen and (max-width: 768px){
  .content{
    padding: 0;
  }
  .logo{
    font-size: 15px;
  }
  .btn-criaTarefa{
    left: 77%;
    width: 47px;
    height: 47px;
    margin: 0 auto;
  }
  .corCriaTarefa{
    display: none;
  }
}

@media only screen and (max-width: 800px){
  .content{
    padding: 0;
  }
  .logo{
    font-size: 15px;
  }

  .btnSetaDireita{
    left: 77%;
  }
  .icon-seta{

  }
}
@media only screen and (max-width: 360px){

  .dataVencimento{
    left: 20%;
  }

  .btn-criaTarefa{
    left: 50%;
    width: 47px;
    height: 47px;
  }
  .logo{
    font-size: 15px;
  }
  .btnSetaDireita{
    left: 50%;
  }
  .spanDataVencimento{
    display: none;
  }
  p{
    overflow: scroll;
  }
}
@media only screen and (max-width: 320px){
  .dataVencimento{
    left: 10%;
  }
  .btn-criaTarefa{
    left: 50%;
    width: 47px;
    height: 47px;
  }
  .logo{
    font-size: 15px;
  }
  .spanDataVencimento{
    display: none;
  }
}
</style>
