<template>
<div class="corpo">
    <h1 class="centralizado">{{ titulo }}</h1>
        <input type="search" class="filtro" placeholder="filtre pelo título da foto">

    <ul class="lista-fotos">
        <li class="lista-fotos-item" v-for="foto of fotosComFiltro" v-bind:key="foto">
            <meu-painel :titulo="foto.titulo">
                <img class="imagem-responsiva" :src="foto.url" :alt="foto.titulo">
            </meu-painel>
        </li>
    </ul>

</div>
</template>

<script>
import Painel from './components/shared/painel/Painel.vue'

export default {

    components: {

        'meu-painel': Painel

    },
    data() {

        return {

            titulo: 'Alurapic',
            fotos: []
        }
    },
    created() {
        this.$http.get('http://localhost:3000/v1/fotos')
            .then(res => res.json())
            .then(fotos => this.fotos = fotos, err => console.log(err))
    },
     computed: {

    fotosComFiltro() {

      if (this.filtro) {
          // criando uma expressão com o valor do filtro, insensitivo
        let exp = new RegExp(this.filtro.trim(), 'i');
        // retorna apenas as fotos que condizem com a expressão
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }

    }
  },

}
</script>

<style>
.centralizado {
    text-align: center;
}

.corpo {
    font-family: Helvetica, sans-serif;
    margin: 0 auto;
    width: 96%;
}

.lista-fotos {
    list-style: none;
}

.lista-fotos .lista-fotos-item {
    display: inline-block;

}

.imagem-responsiva {
    width: 100%;
}
.filtro {
    display: block;
    width: 100%;
  }
</style>

