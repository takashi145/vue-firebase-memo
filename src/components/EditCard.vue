<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
        >
          編集
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          メモを更新
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
            @click="update"
          >
            更新
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import firebase from '@/firebase/firebase'
  export default {
    props: {
      edit_title: {
        type: String,
      },
      edit_body: {
        type: String,
      },
      edit_id: {
        type: Number,
      }
    },
    data () {
      return {
        title: this.edit_title,
        body: this.edit_body,
        id: this.edit_id,
        dialog: false,
      }
    },
    methods: {
      update() {
        this.dialog = false;
        firebase.firestore().collection('cards').doc(this.id).update(
          {
            // date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
            title: this.title,
            body: this.body,
          }
        )
        .then(() => {
          this.title = "";
          this.body = "";
          this.$emit("refresh");
        })
        .catch(error => {
          console.log("fail", error);
        })
        
      }
    }

  }
</script>