<template>
  <q-page class="flex flex-center">
    <ul>
      <q-list bordered separator v-for="historia in numerohistorias" :key="historia.id">
      <q-item clickable v-ripple>
      <q-item-section>{{historia.title}}</q-item-section>
      </q-item>
      <q-item clickable v-ripple>
      <q-item-section>
      <q-item-label overline>Creador</q-item-label>
      <q-item-label>{{historia.by}}</q-item-label>
      </q-item-section>
      </q-item>
      <q-item clickable v-ripple>
      <q-item-section>
      <p>{{historia.text}}</p>
      </q-item-section>
      </q-item>
      <br><br><br>
      </q-list>
      {{numerohistorias}}
    </ul>
  </q-page>
</template>

<script>
import axios from "axios";
export default {
  name: "PageIndex",
  data() {
    return {
      arrayOriginal: [],
      indiceArray: 0,
    }
  },
  methods: {
    update() {
      axios
        .get("https://hacker-news.firebaseio.com/v0/beststories.json")
        .then(response => (this.arrayOriginal = response.data))
        .catch(function(error) {
          // handle error
          console.log(error);
        })
    }
  },
  computed: {
    numerohistorias(){
      var arrayCorto=this.arrayOriginal.slice(this.indiceArray, this.indiceArray+5)
      var arrayDatos=[]
      arrayCorto.forEach(element => {
        console.log(element)
        axios
        .get("https://hacker-news.firebaseio.com/v0/item/"+String(element)+".json?print=pretty")
        .then(response => (arrayDatos.push(response.data)))
        .catch(function(error) {
          // handle error
          console.log(error);
        })
      });
      console.log(arrayDatos)
      return arrayDatos
    }
  },
  created() {
    this.update();
  },

};
</script>
