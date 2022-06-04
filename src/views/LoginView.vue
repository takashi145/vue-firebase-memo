<template>
  <div>
    <h2>Login</h2>
    <v-alert
      dense
      text
      type="success"
      class="mt-3"
      v-if="message"
    >
      {{ message }}
    </v-alert>
    <v-form v-model="valid">
      <v-alert
        dense
        outlined
        type="error"
        class="mt-3"
        v-if="error"
      >
        ログインに失敗しました。
      </v-alert>
      <v-container>
        <v-row>
          <v-col
            cols="12"
            md="8"
            class="mx-auto"
          >
            <v-text-field
              v-model="email"
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
          </v-col>
          <v-col
            cols="12"
            md="8"
            class="mx-auto"
          >
            <v-text-field
              v-model="password"
              :rules="passwordRules"
              label="Password"
              required
              type="password"
            ></v-text-field>
          </v-col>
          <v-col
            cols="12"
          >
            <v-btn
              color="primary"
              @click="login"
            >
              Login
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
              to="/register"
              text
              color="blue"
            >
              新規登録はこちら
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </div>
</template>

<script>
  import firebase from '@/firebase/firebase'
  export default {
    data: () => ({
      valid: false,
      email: '',
      emailRules: [
        v => !!v || '必須項目です。',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ],
      password: '',
      passwordRules: [
        value => !!value || '必須項目です。',
      ],
      error: '',
      message: '',
    }),
    mounted() {
      const message = sessionStorage.getItem('message')
      if(message) {
        this.message = message;
        sessionStorage.removeItem('message');
      }
    },
    methods: {
      login() {
        firebase.auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then((result) => {
          const user = {
            displayName: result.user.displayName,
            email: result.user.email,
            uid: result.user.uid,
            refreshToken: result.user.refreshToken,
          }

          sessionStorage.setItem('user', JSON.stringify(user));
          sessionStorage.setItem('message', 'ログインしました。');
          this.$router.push("/");
        })
        .catch(() => {
          this.error = "ログインに失敗しました。";
        })
      }
    }
  }
</script>

<style>

</style>