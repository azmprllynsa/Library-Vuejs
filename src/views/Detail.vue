<template>
<div class="detail">
    <section class="navbar">
    <div class="detail-nav">
      <div class="nav-back">
        <router-link to="/"><div class="back"></div></router-link>
      </div>
      <div class="nav-option" v-if="this.items.role == '1'">
        <button @click="showModal">Edit</button>
        <button @click="deleteBook">Delete</button>
      </div>
    </div>
    <div class="detail-cover" :style='images'></div>
    <div class="detail-book" :style='images'></div>

    <!-- Modal Edit -->
    <div class="modal">
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Edit Book</p>
          <button class="delete is-large" aria-label="close" @click="showModal"></button>
        </header>
        <section class="modal-card-body">
          <div class="form-login">
            <label for="title">Title</label>
            <input name="Title" id="title" placeholder="Title" v-model="title"/>
          </div>
          <div class="form-login">
            <label for="image">Image</label>
            <input name="image" type="file" id="image"  ref="file" @change="image"/>
          </div>
          <div class="form-login">
            <label for="author">Author</label>
            <input name="author" id="author" placeholder="Author" v-model="author"/>
          </div>
          <div class="form-login">
            <label for="description">Description</label>
            <textarea name="description" id="description" placeholder="Description"
            v-model="description"
            rows="5"/>
          </div>
          <div class="form-login">
            <label for="id_category">Category</label>
            <select name="id_category" id="id_category" v-model="id_category">
              <option value="Novel">Novel</option>
              <option value="Komik">Komik</option>
              <option value="Sci-Fi">Sci-Fi</option>
              <option value="Horror">Horror</option>
              <option value="Biografi">Biografi</option>
            </select>
          </div>
          <div class="form-login">
            <label for="released_date">Released Date</label>
            <input name="released_date" id="released_date" placeholder="Released Date"
            v-model="released_date"/>
          </div>
        </section>
        <footer class="modal-card-foot">
          <button class="button is-warning button-save" @click="editData">Save</button>
        </footer>
      </div>
    </div>
  </section>
  <BodyDetail bookTitle ='Ubur - Ubur Lembur'
  bookAuthor="Raditya Dika"
  bookDescription='Hal kedua yang gue nggak sempat kasih tahu Iman: jadi orang yang dikenal publik
  harus tahan dengan asumsi-asumsi orang. Misalnya, orang-orang penuh dengan asumsi yang salah.
  Gue kurusan dikit, dikomentarin orang yang baru ketemu, ‘Bang Radit, kurusan, deh.
  Buat film baru, ya?’ Gue geleng, ‘Enggak.’ Gue bilang, ‘Emang lagi diet aja.’
  Dia malah balas bilang, ‘Ah, bohong! Paling abis putus cinta, kan?’Giliran gue potong
  rambut botak, ada orang yang ketemu gue di mall nanya, ‘Wah botak sekarang?'/>
</div>
</template>

<script>
import axios from 'axios';
import BodyDetail from '../components/module/BodyDetail.vue';

export default {
  name: 'Detail',
  components: {
    BodyDetail,
  },
  data() {
    return {
      book: [],
      title: '',
      author: '',
      description: '',
      id_category: '',
      released_date: '',
      url: 'http://localhost:8000/api/v1/book/admin/',
    };
  },
  props: ['data'],
  created() {
    this.items = JSON.parse(localStorage.getItem('items'));
    // console.log(this.items);
    if (this.items) {
      this.token = this.items.token;
    }
  },
  methods: {
    getBookById() {
      axios.get(`http://localhost:8000/api/v1/book/${this.$route.params.data}`)
        .then((res) => {
          this.book = res.data.data;
          this.title = res.data.data.title;
          this.author = res.data.data.author;
          this.description = res.data.data.description;
          this.id_category = res.data.data.id_category;
          this.released_date = res.data.data.released_date;
        })
        .catch(() => {
          // console.log(err);
        });
    },
    deleteBook() {
      this.$swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!',
      })
        .then((result) => {
          if (result.value) {
            axios
              .delete(this.url + this.$route.params.data,
                { headers: { Authorization: this.token } })
              .then(() => {
                // console.log(res);
                this.$swal.fire({
                  icon: 'success',
                  html: 'Book has been deleted!',
                  showConfirmButton: false,
                  timer: 3000,
                })
                  .then(() => {
                    this.$router.push('/');
                  });
              })
              .catch(() => {
                // console.log(err);
              });
          }
        });
    },
    editData() {
      const form = new FormData();
      form.append('title', this.title);
      form.append('book_image', this.$refs.file.files[0]);
      form.append('author', this.author);
      form.append('description', this.description);
      form.append('released_date', this.released_date);
      form.append('id_category', this.id_category);
      axios
        .patch(this.url + this.$route.params.data, form,
          { headers: { Authorization: this.token } })
        .then(() => {
          // console.log(res.data);
          this.$router.go();
          this.$swal.fire({
            icon: 'success',
            html: `Book ${this.title} successfully edited!`,
            timer: 6000,
          });
          const modal = document.querySelector('.modal');
          const sidebar = document.querySelector('.sidebar');
          modal.classList.toggle('is-active');
          sidebar.classList.toggle('show-sidebar');
          this.$router.push('/');
          // console.log(res.data);
        })
        .catch(() => {
          // console.log(err);
        });
    },
    showModal() {
      const modal = document.querySelector('.modal');
      modal.classList.toggle('is-active');
    },
  },
  computed: {
    images() {
      return {
        backgroundImage: `url(${this.book.image})`,
      };
    },
  },
  mounted() {
    axios.get(`http://localhost:8000/api/v1/book/${this.$route.params.data}`)
      .then((res) => {
        this.book = res.data.data;
        console.log(this.book.description);
      })
      .catch(() => {
        // console.log(err);
      });
  },
};
</script>

<style scoped>
  .detail-nav{
    width: 100%;
    padding: 10px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .back{
    height: 40px;
    width: 40px;
    background-color: white;
    background-image: url('../assets/img/Arrow.png');
    background-position: center center;
    background-repeat: no-repeat;
    border-radius: 50%;
    cursor: pointer;
  }
  .nav-option button{
    padding: 0 15px;
    background: none;
    border: none;
    font-size: 25px;
    color: #ffffff;
    cursor: pointer;
    outline: none;
    margin-right: 20px;
  }
  .detail-cover{
    width: 100%;
    height: 400px;
    z-index: -1;
    background-color: black;
    position: absolute;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center center;
  }
  .detail-book{
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center center;
    width: 200px;
    height: 300px;
    background-color: salmon;
    position: absolute;
    top: 250px;
    right: 50px;
  }
    /* Modal */
    .modal{
    padding: 0 20px;
  }

  .modal-card-head {
    display: flex;
    flex-direction: row;
    background-color: #fff;
  }
  .modal-card-foot{
    display: flex;
    flex-direction: row-reverse;
    background-color: #fff;
  }
  .form-login{
     display: flex;
     justify-content: space-between;
    align-items: center;
    width: 100%;
    margin-bottom: 10px;
  }
  .form-login label{
    text-align: left;
  }
  .form-login input, .form-login textarea, .form-login select{
    border-radius: 5px;
    width: 80% !important;
    padding: 10px;
    border: 1px solid #d0cccc;
    font-size: 14px;
    background-color: transparent;
  }

  .modal-card-head {
    text-align: left !important;
  }

  textarea{
    resize: none;
}
/* End of Modal */
</style>
