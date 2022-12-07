<script>
import axios from "axios";
import { mapStores, mapState, mapActions } from "pinia";
import { useAuthStore } from "../stores/auth";
import CardArtista from "@/components/CardArtista.vue";
export default {
  data() {
    return {
      dados: {},
      artistas: [],
      path_artistas: [
        {url: "https://api.spotify.com/v1/artists/6ab8tnQvr8sXXIpSZCrbQs", name: "/yunli"},
        {url: "https://api.spotify.com/v1/artists/3ZHU5AKrUmIPnCFfr82QER", name: "/nonly"},
        {url: "https://api.spotify.com/v1/artists/2o8gT0fQmFxGNbowbdgeZe", name: "/dprian"},
        {url: "https://api.spotify.com/v1/artists/0siBQaURCli5wn2lqv8WZg", name: "/dprlive"},
      ]
    };
  },
  components: {CardArtista},
  computed: {
    ...mapStores(useAuthStore),
    ...mapState(useAuthStore, ["token"]),
  },
  methods: {
    ...mapActions(useAuthStore, ["setToken"]),
    async getData() {
      let response = await axios.get("https://api.spotify.com/v1/me", {
        headers: {
          Authorization: `Bearer ${this.token}`,
        },
      });
      this.dados = response.data;

      for (const item of this.path_artistas) {
        response = await axios.get(
          item.url,
          {
            headers: {
              Authorization: `Bearer ${this.token}`,
            },
          }
        );
        this.artistas.push({url: item.name, ...response.data});
      }
    },
  },
  async created() {
    await this.getData();
  },
};
</script>
<template>
  <header>
    <h1>TSIDGE</h1>
    <RouterLink to="/perfil">
      <h2>{{ dados.display_name }}</h2>
    </RouterLink>
  </header>
  <div class="about">
    <div class="artistas">
      <CardArtista v-for="artista of artistas" :key="artista.id" :artista="artista" />
    </div>
  </div>
  <hr />
  <footer></footer>
</template>

<style>
body {
  background-color: #384842;
  color: white;
}

header {
  background-color: #2f413a;
  display: flex;
  padding: 20px;
  box-shadow: rgba(17, 17, 26, 0.05) 0px 4px 16px,
    rgba(17, 17, 26, 0.05) 0px 8px 32px;
}

h1 {
  padding-left: 50px;
  padding-right: 1620px;
  margin-top: 10px;
}

h2 {
  margin-right: 50px;
  margin-top: 10px;
  background-color: black;
  border-radius: 50px;
}

header a {
  text-decoration: none;
  color: white;
}

header a:hover {
  color: #4d6360;
}

.artistas {
  display: flex;
  text-decoration: none;
}

.artista {
  background-color: #4d6360;
  color: white;
  border-radius: 10px;
  width: 400px;
  height: 500px;
  margin: auto;
  margin-top: 100px;
  margin-bottom: 100px;
  box-shadow: rgba(17, 17, 26, 0.05) 0px 4px 16px,
    rgba(17, 17, 26, 0.05) 0px 8px 32px;
}

.artista img {
  margin: 25px 25px 5px;
  box-shadow: rgba(17, 17, 26, 0.05) 0px 4px 16px,
    rgba(17, 17, 26, 0.05) 0px 8px 32px;
}

.artista h3 {
  margin-left: 30px;
}

.artista p {
  margin-left: 30px;
  color: #a1aca8;
}

.artista a {
  text-decoration: none;
  color: white;
  font-size: larger;
}

.artimg {
  max-width: 200px;
  margin: 10px 10px 10px 10px;
}
</style>
