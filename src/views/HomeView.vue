<template>
  <div>
    <CardList />
    <CreateCard />
  </div>
</template>

<script>
import CardList from '@/components/CardList.vue';
import CreateCard from '@/components/CreateCard.vue';

export default {
  name: 'HomeView',
  components: {
    CardList,
    CreateCard
  }
}
</script>

<style>

</style>