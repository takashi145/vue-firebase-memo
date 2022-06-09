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
  export default {
    data () {
      return {
        dialog: false,
      }
    },
    methods: {
      destroy() {
        this.dialog = false;
        this.$emit("delete");
      }
    }

  }
</script>