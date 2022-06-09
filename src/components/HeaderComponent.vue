<template>
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
          text
          color="blue"
        >
          ログアウト
        </v-btn>
        <v-spacer></v-spacer>
        ユーザー： {{ user_name }}
      </v-container>
    </v-app-bar>
</template>

<script>
import firebase from '@/firebase/firebase'
export default {
  data() {
    return {
      user_name: '',
    }
  },
  mounted(){
    const user = JSON.parse(sessionStorage.getItem('user'));
    this.user_name = user['displayName']
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

        })
      }
  }
}
</script>

<style>

</style>