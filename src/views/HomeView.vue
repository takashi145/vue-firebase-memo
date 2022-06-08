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
  </v-app>
</template>

<script>
import CardList from '@/components/CardList.vue';
export default {
  name: 'HomeView',
  components: {
    CardList,
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