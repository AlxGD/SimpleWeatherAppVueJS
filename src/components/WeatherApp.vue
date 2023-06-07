<script>

import axios from 'axios'; //importa a API do axios, instalada com npm install axios
const API_KEY = process.env.VUE_APP_WEATHER_API_KEY; //chama a minha key, que está em um .env particular
export default { //é aqui aonde defino quais vão ser os dados que o WeatherApp.vue vai retornar:
  data() {
    return {
      nomeDaCidade: '',
      dadosDoClima: null,
      dataDaPesquisa: null,
    };
  },
  methods: { //daí aqui decido o método, o que vai fazer.
    async pegarDadosDoClima() { //o método é asíncrono. É como uma função, definida dentro de um componente, no caso, WeatherApp.vue!
      try {
        const response = await axios.get( //esperar o axios para pegar....
            `http://api.weatherapi.com/v1/current.json?key=${API_KEY}&q=${this.nomeDaCidade}&aqi=no` //...a API do weatherapi.com. importante, a minha está no .env local
        ); //notar que o this.nomeDaCidade já está configurado certinho. Vai ser definido lá em baixo, no template. O usuário que coloca o valor

        this.dadosDoClima = { //as transformações! nota que eu criei, dentro de dadosDoClima, três variaveis pra ele: temperature, himidty e rainChance
          temperature: response.data.current.temp_c, //a forma que interage com a API
          humidity: response.data.current.humidity,
          rainChance: response.data.current.precip_mm,
        };

        this.dataDaPesquisa = new Date().toLocaleTimeString();
      } catch (error) {
        console.error(error);
      }
    },
  },
};

//algumas coisas lá no template: na linha 43 que eu atrelo o pegarDadosDoClima ao botão! com um @click=
</script>


<template>
  <div>
    <h1>Como está o Tempo?</h1>
    <div>
      <input v-model="nomeDaCidade" placeholder="Coloque o nome da cidade" />
      <button @click="pegarDadosDoClima">Pesquisar</button>
    </div>
    <div v-if="dadosDoClima">
      <p>Temperatura: {{ dadosDoClima.temperature }}°C</p>
      <p>Humidade: {{ dadosDoClima.humidity }}%</p>
      <p>Chance de Chuva: {{ dadosDoClima.rainChance }}%</p>
      <p>Horário da pesquisa: {{ dataDaPesquisa }}</p>
    </div>
  </div>
</template>


<style>
</style>
