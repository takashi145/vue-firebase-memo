<template>
  <v-container fluid>
    <div class="mb-8">
      <v-row>
        <v-col
          cols="3"
        >
        </v-col>
        <v-col
          cols="6"
        >
          <v-menu
          ref="menu"
          v-model="menu"
          :close-on-content-click="false"
          :return-value.sync="date"
          transition="scale-transition"
          offset-y
          min-width="auto"
        >
          <!-- <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="date"
              label="Picker in menu"
              prepend-icon="mdi-calendar"
              readonly
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
            </template>
            <v-date-picker
              v-model="date"
              no-title
              scrollable
              @change="dateChange"
            >
              <v-spacer></v-spacer>
              <v-btn
                text
                color="primary"
                @click="menu = false"
              >
                Cancel
              </v-btn>
              <v-btn
                text
                color="primary"
                @click="$refs.menu.save(date)"
              >
                OK
              </v-btn>
            </v-date-picker> -->
          </v-menu>
        </v-col>
        <v-col
          cols="3"
        >
        </v-col>
      </v-row>
    </div>
    <v-data-iterator
      :items="items"
      :items-per-page.sync="itemsPerPage"
      hide-default-footer
    >
      
      <template v-slot:default="props">
        <v-row>
          <v-col
            v-for="item in props.items"
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
                      <v-form class="mt-4">
                        <v-textarea
                          name="input-7-1"
                          label="メモ"
                          :value="item.body"
                          hint="100文字以内"
                        >
                        </v-textarea>
                      </v-form>
                  </v-list-item-content>
                </v-list-item>
                <v-divider></v-divider>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="error"
                    class="mr-4"
                    @click="delete_card(item.id)"
                  >
                    削除
                  </v-btn>
                </v-card-actions>
            </v-card>
            
          </v-col>
        </v-row>
      </template>
    </v-data-iterator>

  </v-container>
</template>

<script>
import firebase from '@/firebase/firebase'
  
  export default {
    data: () => ({
      menu: false,
      items: [],
      deleteId: '',
    }),
    async created() {
        this.dataReset()
    },
    methods: {
      delete_card(id) {
        console.log(this.items);
        firebase.firestore().collection('cards').doc(id).delete();
        this.dataReset()
      },

      // async dateChange() {
      //   const cardRef = firebase.firestore().collection("cards")
      //   const snapshot = await cardRef.get();
      //   this.items = [];
      //   snapshot.forEach(doc => {
      //     this.items.push(doc.data())
      //   })
      // },

      async dataReset() {
        const cardRef = firebase.firestore().collection("cards");
        const snapshot = await cardRef.get();
        this.items = []
        snapshot.forEach(doc => {
          const data = doc.data()
          data.id = doc.id
          this.items.push(data)
        })
      }

    },
  }
</script>
