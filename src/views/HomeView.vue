<template>
  <v-container fluid>
    
    <v-data-iterator
      :items="items"
      :items-per-page.sync="itemsPerPage"
      hide-default-footer
    >
      
      <template v-slot:default="props">
        <v-row>
          <v-col
            v-for="item in props.items"
            :key="item.name"
            cols="12"
            sm="6"
            md="4"
            lg="3"
          >
            <v-card
              class="pa-2"
            >
              <v-card-title class="subheading font-weight-bold">
                {{ item.name }}
              </v-card-title>

              <v-divider></v-divider>

              <v-list dense>
                <v-list-item>
                  <v-list-item-content>支出:</v-list-item-content>
                  <v-list-item-content class="align-end">
                    <template>
                      <v-text-field 
                        :rules="rules"
                        :value="item.spending"
                      >
                      </v-text-field>
                    </template>
                  </v-list-item-content>
                </v-list-item>

                <v-divider></v-divider>
                <v-list-item>
                  <v-list-item-content>メモ:</v-list-item-content>
                  <v-list-item-content>
                    <template>
                      <v-form class="mt-4">
                        <v-textarea
                          v-model="title"
                          counter
                          maxlength="100"
                          full-width
                          single-line
                          filled
                        ></v-textarea>
                      </v-form>
                    </template>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-card>
          </v-col>
        </v-row>
      </template>
    </v-data-iterator>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      items: [
        {
          name: '食費',
          spending: 100,
          memo: 'aaaaaaaaaa'
        },
        {
          name: '衣服',
          spending: 100,
          memo: 'bbbbbbbbb'
        },
        {
          name: '光熱費',
          spending: 100,
          memo: 'ccccccccccc'
        },
        {
          name: 'その他',
          spending: 100,
          memo: 'dddddddddddd'
        },
      ],
    }),
  }
</script>
