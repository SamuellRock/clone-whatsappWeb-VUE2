<template>
  <div id="app">
    <section class="section">
      <div class="container">
        <div class="columns janela-principal">

          <!-- Coluna Esquerda -->
          <div class="column is-3 lista-de-conversas">
            <div class="barra-superior"  />
            <item-lista-de-conversa 
              v-for="(conversa, indice) in conversas"
              :key="indice" 
              :mensagemAtiva="indice == indiceAtivo"
              :nomeDoUsuario="conversa.usuario"
              :ultimaMensagem="conversa.mensagens[0].conteudo"
              v-on:click.native="indiceAtivo=indice"
            />
          </div>

          <!-- Coluna Direita -->
          <div class="column conversa-ativa">
            <!-- 1. Barra Superior -->
            <div class="barra-superior">
              <span>{{conversas[indiceAtivo].usuario}}</span>
            </div>
            <!-- 2. Mensagens -->
            <div class="lista-mensagens">
              <mensagem-da-conversa-ativa 
                :conteudo="mensagem.conteudo"
                :horario="mensagem.horario"
                :verde="mensagem.verde"
                v-for="(mensagem, indice_) in conversas[indiceAtivo].mensagens"
                :key="indice_" 
              />
            </div>
            <!-- 3. Barra Inferior -->
            <div class="barra-inferior">
              <input type="text" class="input"  v-on:keyup.enter="enviarMensagem" placeholder="Insira sua mensagem" v-model="conteudoASerEnviado">
            </div>
          </div>

        </div>
      </div>
    </section>
  </div>
</template>

<script>
import conversasIniciais from "./dados.js"
import ItemDaListaDeConversa from './components/ItemDaListaDeConversa.vue';
import MensagemDaConversaAtiva from './components/MensagemDaConversaAtiva.vue';
export default {
  name: "App",
  data: function(){
    return {
        conversas: conversasIniciais,
        indiceAtivo: 0,
        conteudoASerEnviado: "",
      }
  },
  components: {
    "item-lista-de-conversa": ItemDaListaDeConversa,
    "mensagem-da-conversa-ativa": MensagemDaConversaAtiva
  },
  methods: {
    enviarMensagem: function(){
      let horarioAtual = new Date().getHours() + ":" + new Date().getMinutes();

      let novaMensagem = {
        horario: horarioAtual,
        conteudo: this.conteudoASerEnviado,
        verde: true
      };

      this.conversas[this.indiceAtivo]
        .mensagens
        .push(novaMensagem);

      this.conteudoASerEnviado = "";
    }
  }

};
</script>

<style>
.janela-principal{
    height: 95vh;
    box-shadow: 0 3rem 3rem -1rem rgba(10,10,10,.2);
}
/* Barra Superior */
.barra-superior{
    margin: 0;
    height: 50px;
    background: #EDEDED;
    border-right: 1px solid #E1E1E1;
    border-bottom: 1px solid rgb(200, 200, 200);
}
.barra-superior span{
    line-height: 50px;
    margin-left: 25px;
    font-weight: 500;
}

/* Coluna Esquerda */
.lista-de-conversas{
    padding: 0;
    background:white;
}

/* Coluna Direita */
.conversa-ativa{
    padding: 0;
    background:#E5DDD5;
    position: relative;
}
.lista-mensagens{
    height: 75vh;
    display: flex;
    justify-content: flex-end;
    flex-direction: column;
}

/* Barra Inferior */
.barra-inferior{
    bottom:0;
    width: 100%;
    padding: 10px;
    position: absolute;
    background: #f0f0f0;
}
.barra-inferior input{
    border:none;
    padding: 10px;
    margin: 0 50px;
    width: 90%;
    border-radius: 15px;
    font-size: 16px;
}
</style>
