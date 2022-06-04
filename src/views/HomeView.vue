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
    <CardList />
    <CreateCard />
  </v-app>
</template>

<script>
import CardList from '@/components/CardList.vue';
import CreateCard from '@/components/CreateCard.vue';

export default {
  name: 'HomeView',
  components: {
    CardList,
    CreateCard
  },
  data() {
    return {
      message: '',
    }
  },
  mounted() {
    const message = sessionStorage.getItem('message');
    if(message) {
      this.message = message;
      sessionStorage.removeItem('message');
    }
  }
}
</script>

<style>

</style>