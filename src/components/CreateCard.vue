<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="blue lighten-2"
          dark
          v-bind="attrs"
          v-on="on"
        >
          メモを追加
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 lighten-2">
          新しいメモ
        </v-card-title>

        <v-divider></v-divider>
        <v-form v-model="valid">
          <v-row>
            <v-col
              cols="12"
            >
              <v-text-field
                v-model="title"
                label="タイトル"
                :rules="titleRules"
                required
                class="mx-2"
              ></v-text-field>
            </v-col>

            <v-col
              cols="12"
            >
              <v-textarea
                v-model="body"
                label="メモ"
                :rules="bodyRules"
                required
                class="mx-2"
              >
              </v-textarea>
            </v-col>
          </v-row>

          <v-card-actions>
            <v-btn
              color="secondary"
              class="mr-4"
              @click="dialog=false"
            >
              キャンセル
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn
              :disabled="!valid"
              color="primary"
              class="mr-4"
              @click="add"
            >
              追加
            </v-btn>
          </v-card-actions>
        </v-form>
        
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import firebase from '@/firebase/firebase'
  export default {
    name: 'CreateCard',
    data () {
      return {
        valid: false,
        title: '',
        body: '',
        dialog: false,
        titleRules: [
          v => !!v || '必須項目です。',
          v => v.length <= 30 || 'タイトルは30文字以内で入力してください。',
        ],
        bodyRules: [
          v => !!v || '必須項目です。',
          v => v.length <= 100 || '説明は100文字以内で入力してください。',
        ],
      }
    },
    methods: {
      add() {
        this.dialog = false;
        const user = JSON.parse(sessionStorage.getItem('user'));
        firebase.firestore().collection('cards').add(
          {
            title: this.title,
            body: this.body,
            user_id: user['uid'],
          }
        )
        .then(() => {
          this.title = "";
          this.body = "";
          this.$emit("refresh");
        })
        .catch(error => {
          console.log("error", error);
        })
      }
    }
  }
</script>