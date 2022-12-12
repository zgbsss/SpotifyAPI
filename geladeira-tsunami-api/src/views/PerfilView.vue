<script>
import axios from "axios";
import { mapStores, mapState } from "pinia";
import { useAuthStore } from "../stores/auth";
export default {
  data() {
    return {
      dados: {
        images: [
          {
            url: "",
          },
        ],
      },
    };
  },
  computed: {
    ...mapStores(useAuthStore),
    ...mapState(useAuthStore, ["token"]),
  },
  methods: {
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
    console.log(this.token);
    let response = await axios.get("https://api.spotify.com/v1/me", {
      headers: {
        Authorization: `Bearer ${this.token}`,
      },
    });
    this.dados = response.data;
  },
};
</script>

<template>
  <div class="container">
    <header>
      <img :src="dados.images[0].url" />
      <h1>{{ dados.display_name }}</h1>
    </header>
    <main>
      <p>{{ dados.email }}</p>
      <p>{{ dados.product }}</p>
    </main>
    <hr />
    <footer>
      <p>{{ token }}</p>
    </footer>
  </div>
</template>

<style>
header {
  background-color: #26312d;
  display: flex;
}
header img {
  height: 300px;
  width: 300px;
  border-radius: 50%;
  box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px,
    rgba(0, 0, 0, 0.22) 0px 15px 12px;
}

.container {
  background: linear-gradient(0, #26312d, #384842);
  color: white;
}
</style>
