<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="error"
          dark
          v-bind="attrs"
          v-on="on"
        >
          削除
        </v-btn>
      </template>

      <v-card>
        <v-card-title
          class="text-h5 red lighten-2"
        >
          メモを削除
        </v-card-title>

        <v-divider></v-divider>
        <div
          class="my-4"
        >
          削除すると元に戻すことはできません。本当に削除しますか？
        </div>
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
            color="error"
            class="mr-4"
            @click="destroy()"
          >
            削除
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import firebase from '@/firebase/firebase'
  export default {
    name: 'DeleteCard',
    props: {
      delete_id: {
        type: String,
      }
    },
    data () {
      return {
        id: this.delete_id,
        dialog: false,
      }
    },
    methods: {
      destroy() {
        this.dialog = false;
        firebase.firestore().collection('cards').doc(this.id).delete()
        .then(() => {
          this.$emit("refresh");
        })
        .catch(error => {
          console.log("fail", error);
        })
        
      }
    }

  }
</script>