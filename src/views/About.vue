<template>
  <div>
    <h1>{{title}}</h1>
    <p> {{editDescription}} </p>
    <h4>nama Kategori :{{ category}}</h4>
    <h5>jumlah buku: {{jumlah}}</h5>
    <button v-on:click="updateTitle()">ubah kategoru</button>
  </div>
</template>
<script>
export default {
  name: 'Navbar',
  props: ['title', 'description'],
  data() {
    return {
      category: 11,
      count: 0,
    };
  },
  methods: {
    updateTitle() {
      this.count += 1;
    },
    // updateKategori(data) {
    //   this.category = data;
    // },
  },
  computed: {
    jumlah() {
      return this.count * 2;
    },
    // editDescription() {
    //   return `Description is : ${this.description}`;
    // },
  },
};
</script>
<style scoped>

</style>
