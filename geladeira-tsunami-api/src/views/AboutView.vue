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

    console.log(this.token);

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
    <h2>{{ dados.display_name }}</h2>
  </header>
  <hr>
  <div class="about">
    <div class="artistas">
      <div class="artista">
        <RouterLink to="/yunli">
          <img class="artimg" src="public/yunli.jpg" alt="" />
          {{ yunli.name }}
          {{ yunli.genres }}
          {{ yunli.followers }}
        </RouterLink>
      </div>
      <div class="artista">
        <RouterLink to="/nonly">
          <img class="artimg" src="public/download.jpeg" alt="" />
          {{ nonly.name }}
          {{ nonly.genres }}
          {{ nonly.followers }}
        </RouterLink>
      </div>
      <div class="artista">
        <RouterLink to="/dprian">
          <img class="artimg" src="public/dprian.jpeg" alt="" />
          {{ dprian.name }}
          {{ dprian.followers }}
          {{ dprian.genres }}
        </RouterLink>
      </div>
      <div class="artista">
        <RouterLink to="/dprlive">
          <img src="public/dprlive.jpeg" alt="" class="artimg" />
          {{ dprlive.name }}
          {{ dprlive.followers }}
          {{ dprlive.genres }}
        </RouterLink>
      </div>
    </div>
  </div>
  <hr>
  <footer>
    <p>{{ token }}</p>
  </footer>
</template>

<style>

body {
  background-color: #2f413a;
  color: white;
}

.artistas {
  display: flex;
  text-decoration: none;
}

.artista {
  background-color: #4d6360;
  color: white;
  border-radius: 10px;
  width: 300px;
  height: 400px;
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
  color: #2f413a;
}

.artimg {
  max-width: 200px;
  margin: 10px 10px 10px 10px;
}
</style>
