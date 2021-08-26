<template>
  <div class="container text-center">
    <img alt="Vue logo" src="../assets/img-pokemon.png">
    <h1 class="mt-3">PokeGuía</h1>

    <form action="" class="mt-4">
      <label for="name">Nombre:</label>
      <input id="name" class="ms-3" type="text" name="name" minlength="1" required v-model="pokemon.name">
      <button class="ms-3 btn btn-primary" type="submit" v-on:click="searchPoke($event)">Buscar</button>
    </form>

    <div id="response">
      <div id="imagen" class="mt-4">
        <img :src="imagePoke" alt="img-pokemon" />
      </div>
      <div id="moves">
        <h3 class="mt-3">{{titleMoves}}</h3>
        <ul class="d-inline-block">
          <li class="text-start" v-for="(item, index) in movesPoke" :key="index">{{item.move.name}}</li>
        </ul>
      </div>
      <div id="abilities">
        <h3 class="mt-3">{{titleAbilities}}</h3>
        <ul class="d-inline-block">
          <li class="text-start" v-for="(item, index) in abilitiesPoke" :key="index">{{item.ability.name}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'PokeGuia',
    data() {
      return {
        BASE_URL: "https://pokeapi.co/api/v2/pokemon/",
        titleMoves: '',
        titleAbilities: '',
        pokemon: {
          name: 'pikachu',
          sprites: {
            front_default: ''
          },
          moves: [],
          abilities: []
        },
      }
    },
    created() {
      this.searchPoke(0);
    },
    methods: {
      searchPoke(event) {
        if (event != 0)
          event.preventDefault();

        console.log(this.pokemon.name)
        fetch(`${this.BASE_URL}${this.pokemon.name}`)
          .then(response => response.json())
          .then(json => {
            this.titleMoves = "Movimientos";
            this.titleAbilities = "Habilidades";
            this.pokemon = json;
          }).catch(err => {
            console.log(err)
            alert(`pokemon "${this.pokemon.name}" no encontrado.`);
            document.getElementById('name').value="";
          });
      }
    },
    computed: {
      imagePoke() {
        return this.pokemon.sprites.front_default;
      },
      movesPoke() {
        return this.pokemon.moves;
      },
      abilitiesPoke() {
        return this.pokemon.abilities;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  div {
    color: rgb(9, 9, 100);
  }

  button:hover {
    color: #ffc107 !important;
  }

  img {
    max-width: 250px;
    max-height: 250px;
  }
</style>