<script>
import axios from "axios";
import { mapStores, mapState, mapActions } from "pinia";
import { useAuthStore } from "../stores/auth";
export default {
  data() {
    return {
      dados: {},
      yunli: {},
      nonly: {},
      dprian: {},
      dprlive: {},
    };
  },
  computed: {
    ...mapStores(useAuthStore),
    ...mapState(useAuthStore, ["token"]),
  },
  methods: {
    ...mapActions(useAuthStore, ["setToken"]),
    getHashParams() {
      var hashParams = {};
      var e,
        r = /([^&;=]+)=?([^&;]*)/g,
        q = this.token;
      e = r.exec(q);
      while (e) {
        hashParams[e[1]] = decodeURIComponent(e[2]);
        e = r.exec(q);
      }
      return hashParams;
    },
  },
  async created() {
    this.setToken(this.$route.query["access_token"]);

    // console. log(this.token);

    let response = await axios.get("https://api.spotify.com/v1/me", {
      headers: {
        Authorization: `Bearer ${this.token}`,
      },
    });
    this.dados = response.data;

    response = await axios.get(
      "	https://api.spotify.com/v1/artists/6ab8tnQvr8sXXIpSZCrbQs",
      {
        headers: {
          Authorization: `Bearer ${this.token}`,
        },
      }
    );
    this.yunli = response.data;

    response = await axios.get(
      "	https://api.spotify.com/v1/artists/3ZHU5AKrUmIPnCFfr82QER",
      {
        headers: {
          Authorization: `Bearer ${this.token}`,
        },
      }
    );
    this.nonly = response.data;

    response = await axios.get(
      "https://api.spotify.com/v1/artists/2o8gT0fQmFxGNbowbdgeZe",
      {
        headers: {
          Authorization: `Bearer ${this.token}`,
          Host: "api.spotify.com",
        },
      }
    );
    this.dprian = response.data;

    response = await axios.get(
      "https://api.spotify.com/v1/artists/0siBQaURCli5wn2lqv8WZg",
      {
        headers: {
          Authorization: `Bearer ${this.token}`,
          Host: "api.spotify.com",
        },
      }
    );
    this.dprlive = response.data;
    // console.log(this.getHashParams());
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
      <div class="artista" v-if="yunli.images.length > 0">
        <RouterLink to="/yunli">
          <img :src="yunli.images[0].url" width="300" height="300">
          {{ yunli.name }}
          {{ yunli.genres }}
          {{ yunli.followers }}
        </RouterLink>
      </div>
      <div class="artista" v-if="nonly.images.length > 0">
        <RouterLink to="/nonly">
          <img :src="nonly.images[0].url" width="300" height="300">
          {{ nonly.name }}
          {{ nonly.genres }}
          {{ nonly.followers }}
        </RouterLink>
      </div>
      <div class="artista" v-if="dprian.images.length > 0">
        <RouterLink to="/dprian">
          <img :src="dprian.images[0].url" width="300" height="300">
          {{ dprian.name }}
          {{ dprian.followers }}
          {{ dprian.genres }}
        </RouterLink>
      </div>
      <div class="artista" v-if="drplive.images.length > 0">
        <RouterLink to="/dprlive">
          <img :src="dprlive.images[0].url" width="300" height="300">
          {{ dprlive.name }}
          {{ dprlive.followers }}
          {{ dprlive.genres }}
        </RouterLink>
      </div>
    </div>
  </div>
  <hr>
  <footer>
  </footer>
</template>

<style>
body {
  background-color: #384842;
  color: white;
}
header {
  background-color: #2F413A;
  display: flex;
  padding: 20px;
}
h1 {
  padding-left: 50px;
  padding-right: 1000px;
}
h2 {
  padding-left: 620px;
  padding-right: 50px;
}
header a {
  text-decoration: none;
  color: white;
}
header a:hover {
  color: #4D6360;
}


.artistas {
  display: flex;
  text-decoration: none;
}
.artista {
  background-color: #4D6360;
  color: white;
  border-radius: 10px;
  width: 400px;
  height: 500px;
  margin: auto;
  margin-top: 100px;
  margin-bottom: 100px;
}
.artista a {
  text-decoration: none;
  color: white;
  font-size: larger;
}
.artista a:hover {
  color: #384842;
}
.artimg {
  max-width: 200px;
  margin: 10px 10px 10px 10px;
}
</style>
