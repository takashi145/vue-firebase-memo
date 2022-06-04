<template>
  <div>
    <h2>新規登録</h2>
    <v-form v-model="valid">
      <v-alert
        dense
        outlined
        type="error"
        class="mt-3"
        v-if="error"
      >
        {{ error }}
      </v-alert>
      <v-container>
        <v-row>
          <v-col
            cols="12"
            md="8"
            class="mx-auto"
          >
            <v-text-field
              v-model="name"
              :rules="nameRules"
              label="Name"
              required
            ></v-text-field>
          </v-col>
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
              :disabled="!valid"
              color="primary"
              @click="register"
            >
              新規登録
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
              to="/login"
              text
              color="blue"
            >
              ログインはこちら
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
      name: '',
      nameRules: [
        v => !!v || '名前を入力してください。',
        v => v.length <= 30 || '名前は30文字以内です。',
      ],
      email: '',
      emailRules: [
        v => !!v || 'メールアドレスを入力してください。',
        v => /.+@.+/.test(v) || 'メールアドレスが不正です。',
      ],
      password: '',
      passwordRules: [
        v => !!v || 'パスワードを入力してください。',
        v => v.length >= 8 || 'パスワードは8文字以上です。',
      ],
      error: '',
    }),
    methods: {
      register() {
        firebase.auth()
        .createUserWithEmailAndPassword(this.email, this.password)
        .then(async (result) => {
          await result.user.updateProfile(
            {displayName: this.name}
          );
          sessionStorage.setItem('message', '新規登録に成功しました。')
          this.$router.push("/login")
        })
        .catch(() => {
          this.error = "ユーザー新規登録に失敗しました。"
        })
      }
    }
  }
</script>

<style>

</style>