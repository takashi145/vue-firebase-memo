<template>
  <v-app>
    <header-component />
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
            <create-card @add="add" />
          </v-col>
        </v-row>
      </div>
          <v-row>
            <v-col
              v-for="item in items"
              :key="item.id"
              cols="12"
              sm="12"
              md="6"
              lg="4"
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
                      @update="update($event, item.id)"
                    />
                    <v-spacer></v-spacer>
                    <delete-card
                      @delete="destroy(item.id)"
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
  import HeaderComponent from '@/components/HeaderComponent.vue';
  
  export default {
    name: 'home-view',
    components: {
      CreateCard,
      EditCard,
      DeleteCard,
      HeaderComponent,
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
      add(title, body) {
        const user = JSON.parse(sessionStorage.getItem('user'));
        firebase.firestore().collection('cards').add(
          {
            title: title,
            body: body,
            user_id: user['uid'],
          }
        )
        .then(() => {
          this.dataReset();
        })
        .catch(() => {
        })
      },

      update(memo, id) {
        firebase.firestore().collection('cards').doc(id).update(
          {
            title: memo.title,
            body: memo.body,
          }
        )
        .then(() => {
          this.dataReset();
        })
        .catch(() => {
        })
      },

      destroy(id) {
        firebase.firestore().collection('cards').doc(id).delete()
        .then(() => {
          this.dataReset();
        })
        .catch(() => {
        })
      },

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