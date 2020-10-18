<template>
  <div>
      <h1>Countdown</h1>
      <h2>{{ formatTime(tiempo) }}</h2>
      <button v-for="valores in botones" :key="valores" @click="countdown(valores.tiempo)">{{valores.etiqueta}}</button>
      <br>
        <div v-if="tiempo > 0">
          <button @click="pausar(estado.activo)">{{estado.boton}}</button>
          <button @click="tiempo = 0">Reiniciar</button>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Countdown',
  data(){
    return{
      tiempo : 0,
        botones: [
            {etiqueta: '03s'  , tiempo: 3 },
            {etiqueta: '01m'  , tiempo: 60 }, 
            {etiqueta: '05m'  , tiempo: 300 }, 
            {etiqueta: '10m' , tiempo: 600 }, 
            {etiqueta: '30m' , tiempo: 1800 },  
        ],
        intervalo : '',
     estado: {activo: false, boton: 'Iniciar'},
    }
  },
  methods: {
      formatTime: function (tiempo){
          let minutos = ('0' + Math.floor(tiempo/60)).slice(-2);
          let segundos = ('0' + tiempo%60).slice(-2);
          return `${minutos}:${segundos} min/seg`
      },
      countdown: function (count){
          clearInterval(this.intervalo)
          this.tiempo = count;
            this.estado.activo = true;
          this.estado.boton = 'Pausar';
          this.intervalo = setInterval(()=>{
              if (this.tiempo > 0) {
                  this.tiempo --; 
              } else {
                  clearInterval(this.intervalo)
              }
          } , 1000);
          
      },
        pausar: function(active){
          if(active){
              clearInterval(this.intervalo);
              this.estado.activo = false;
              this.estado.boton = 'Iniciar';
          }else if(this.tiempo > 0){
              this.countdown(this.tiempo);
          }
      }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  
</style>