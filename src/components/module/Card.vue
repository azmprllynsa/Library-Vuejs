<template>
  <div class="card">
    <div class="card-image">
      <figure class="image is-2by2">
        <img :src="image" :alt="image">
      </figure>
    </div>
    <div class="card-content">
      <div class="media">
        <div class="media-content">
          <p class="title is-4">{{ bookTitle }}</p>
        </div>
      </div>
      <div class="content">
        {{ bookAuthor }}
      </div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'Card',
  props: [
    'image',
    'bookTitle',
    'bookAuthor',
  ],
};
</script>


<style scoped>
  .card{
    transition: 0.5s;
    width: 250px;
    height: 250px;
    margin: 15px;
    border-radius: 15px;
  }
  .card-image{
    height: 150px;
    overflow: hidden;
    border-top-left-radius: 15px;
    border-top-right-radius: 15px;
  }
  .title{
    font-size: 18px;
  }
  .media-content{
    height: 20px;
    /* overflow: hidden; */
  }
  .content{
    font-size: 14px;
    /* height: 85px; */
    overflow: hidden;
  }
  .card-content{
    padding: 10px 20px;
  }
  .card-content .media{
    margin-bottom: 16px !important;
  }
</style>
