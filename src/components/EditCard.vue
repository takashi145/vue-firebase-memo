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
              @click="update"
            >
              更新
            </v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  export default {
    props: {
      edit_title: {
        type: String,
      },
      edit_body: {
        type: String,
      },
    },
    data () {
      return {
        valid: false,
        title: this.edit_title,
        body: this.edit_body,
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
      update() {
        this.dialog = false;
        this.$emit("update", {title: this.title, body: this.body});
        
      }
    }

  }
</script>