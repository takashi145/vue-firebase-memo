<template>
  <v-app>
    <v-alert
      dense
      text
      type="success"
      class="mt-3"
      v-if="message"
    >
      {{ message }}
    </v-alert>

    <v-container fluid>
      <div class="mb-4">
        <v-row>
          <v-col
            cols="12"
            class="text-h5"
          >
            メモ数: {{ items.length }}
          </v-col>
        </v-row>
        <v-row>
          <v-col
            cols="12"
          >
            <create-card @refresh="dataReset" />
          </v-col>
        </v-row>
      </div>
          <v-row>
            <v-col
              v-for="item in items"
              :key="item.id"
              cols="12"
              sm="6"
              md="4"
              lg="3"
            >
              <v-card
                class="pa-2"
              >
                <v-card-title class="subheading font-weight-bold">
                  {{ item.title }}
                </v-card-title>

                <v-divider></v-divider>

                  <v-divider></v-divider>
                  <v-list-item>
                    <v-list-item-content>
                        <div class="mt-4">
                          <v-textarea
                            label="メモ"
                            :value="item.body"
                            readonly
                          >
                          </v-textarea>
                        </div>
                    </v-list-item-content>
                  </v-list-item>
                  <v-divider></v-divider>
                  <v-card-actions>
                    <edit-card
                      :edit_title="item.title" 
                      :edit_body="item.body" 
                      :edit_id="item.id"
                      @refresh="dataReset"
                    />
                    <v-spacer></v-spacer>
                    <delete-card
                      :delete_id="item.id"
                      @refresh="dataReset"
                    />
                  </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
    </v-container>
  </v-app>
</template>

<script>
  import firebase from '@/firebase/firebase'
  import CreateCard from '@/components/CreateCard.vue';
  import EditCard from '@/components/EditCard.vue';
  import DeleteCard from '@/components/DeleteCard.vue';
  
  export default {
    name: 'home-view',
    components: {
      CreateCard,
      EditCard,
      DeleteCard,
    },
    data() {
      return {
        items: [],
        message: '',
      }
    },
    mounted() {
      const message = sessionStorage.getItem('message');
      if(message) {
        this.message = message;
        sessionStorage.removeItem('message');
      }
    },
    created() {
        this.dataReset()
    },
    methods: {
      async dataReset() {
        const user = JSON.parse(sessionStorage.getItem("user"));
        const uid = user["uid"];
        const cardRef = firebase.firestore().collection("cards").where("user_id", "==", uid);
        const snapshot = await cardRef.get();
        this.items = []
        snapshot.forEach(doc => {
          this.items.push({...doc.data(), id: doc.id})
        })
      }
    },
  }
</script>

<style>

</style>