<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="blue lighten-2 "
          dark
          v-bind="attrs"
          v-on="on"
        >
          +
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          新しいメモ
        </v-card-title>

        <v-divider></v-divider>

        <v-row>
          <v-col
            cols="12"
          >
            <v-text-field
              v-model="title"
              label="タイトル"
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
              required
              class="mx-2"
            >
            </v-textarea>
          </v-col>
        </v-row>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            class="mr-4"
            @click="add"
          >
            追加
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import firebase from '@/firebase/firebase'
  export default {
    data () {
      return {
        title: '',
        body: '',
        dialog: false,
      }
    },
    methods: {
      add() {
        firebase.firestore().collection('cards').add(
          {
            // date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
            title: this.title,
            body: this.body,
            user_id: 1,
          }
        )
        .then(result => {
          console.log("success", result);
        })
        .catch(error => {
          console.log("fail", error);
        })
        this.$router.push('/cards');
      }
    }
  }
</script>