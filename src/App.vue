<template>
  <v-app>
    <v-app-bar
      app
      color="white"
    >
      <v-container class="py-0 fill-height">
        <v-app-bar-nav-icon
          color="black"
        >
          メモアプリ
        </v-app-bar-nav-icon>
        <v-btn
          text
          to="/"
          class="ml-16"
          color="black"
        >
          ホーム
        </v-btn>

        <v-btn
          @click="logout"
          v-if="show"
          text
          color="blue"
        >
          ログアウト
        </v-btn>
      </v-container>
    </v-app-bar>

    <v-main>
      <v-container class="mt-8">
        <router-view/>
      </v-container>
    </v-main>

  </v-app>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>

<script>
  import firebase from '@/firebase/firebase'
  export default {
    data () {
      return {
        show: true
      }
    },
    methods: {
      logout() {
        firebase.auth()
        .signOut()
        .then(() => {
          sessionStorage.removeItem('user');
          sessionStorage.setItem('message', "ログアウトしました。");
          this.$router.push("/login")
        })
        .catch(() => {
          console.log("error");
        })
      }
    }
  }
</script>
