<template>
  <HomeWrapper class="home">
    <EntradaWrapper>
      <v-text-field
        :value="entrada"
        @input="handleChange"
        placeholder="Digite algo..."
        clearable 
        outline 
        color="#455a64"
      />
    </EntradaWrapper>
    <div class="saida">
      <p>
        {{ entrada }}
      </p>
    </div>
  </HomeWrapper>
</template>

<script>

import { Subject } from "rxjs";
import { debounceTime, distinctUntilChanged } from "rxjs/operators";

import { HomeWrapper, EntradaWrapper } from './HomeStyled.js';

export default {
  components: {
    HomeWrapper,
    EntradaWrapper
  },
  data() {
    return {
      searchRxjs: new Subject(),
      subscriptionSearch: null,
      entrada: ''
    }
  },
  mounted() {
    // Espera 700ms após o usuário digitar e aí dispara o evento com o valor digitado.
    this.subscriptionSearch = this.searchRxjs.pipe(
      debounceTime(700),
      distinctUntilChanged(),
    )
    .subscribe(this.getObsPage());
  },

  methods: {
    // Funções para o Observable do input de pesquisa.
    getObsPage() {
      let obs = {
        next: (data) => {
          // console.log(data.name, data.value);
          this.entrada = data;
        },
      }
      return obs;
    },
    handleChange(value) {
      // console.log(name, event.target.value);
      // const value = event.target.value;
      this.searchRxjs.next(value);
    },
  }
}
</script>

<style scoped>
  .saida {
    height: 40px;
  }
</style>
