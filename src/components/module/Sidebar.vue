<template>
  <section class="sidebar">
    <div class="menu-button">
      <button class="sidebar-button" v-on:click="sidebarHide">
        <img src="../../assets/img/menu.png" alt="toggle-menu" />
      </button>
    </div>
    <div class="profile">
      <img
        src="../../assets/img/default-user.jpg"
        alt="Default User"
      />
      <h4>{{this.items.name}}</h4>
    </div>
    <div class="column search">
      <div class="search-box">
        <i class="fas fa-search"></i>
        <input
          class="form-control mr-sm-2"
          type="search"
          placeholder="Search..."
          aria-label="Search"/>
      </div>
    </div>
    <div class="sidebar-menu">
      <p><router-link to="/" class="menu-list">Explore</router-link></p>
      <p><router-link to="/history" class="menu-list">History</router-link></p>
      <p data-toggle="modal" @click="showModal" v-if="this.items.role == '1'">Add Book*</p>
      <!-- <p class="none" v-else>Add Book*</p> -->
      <p @click="logout">Logout</p>
    </div>

    <!-- Modal Add -->
    <div class="modal">
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Add Book</p>
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
            <!-- <input name="id_category" id="id_category" placeholder="Category"
            v-model="id_category"/> -->
            <select name="id_category" id="id_category" placeholder="Choose Category"
            v-model="id_category">
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
          <button class="button is-warning button-save" @click="addBook">Save</button>
        </footer>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Sidebar',
  components: {
    // Modal,
  },
  data() {
    return {
      title: null,
      image: null,
      author: null,
      description: null,
      released_date: null,
      id_category: null,
      name: null,
      role: null,
      items: [],
      token: '',
    };
  },
  created() {
    this.items = JSON.parse(localStorage.getItem('items'));
    if (this.items) {
      this.token = this.items.token;
    }
  },
  mounted() {
    this.getUserById();
  },
  methods: {
    logout() {
      localStorage.removeItem('items');
      this.$router.go();
    },
    sidebarHide() {
      const sidebar = document.querySelector('.sidebar');
      sidebar.classList.toggle('show-sidebar');
    },
    addBook() {
      console.log(this.$refs.file);
      const form = new FormData();
      form.append('title', this.title);
      form.append('book_image', this.$refs.file.files[0]);
      form.append('author', this.author);
      form.append('description', this.description);
      form.append('released_date', this.released_date);
      form.append('id_category', this.id_category);
      axios
        .post('http://localhost:8000/api/v1/book/admin', form,
          {
            headers: {
              Authorization: this.token,
            },
          })
        .then((res) => {
          console.log(res.data);
          this.$router.go();
          this.$swal.fire({
            icon: 'success',
            html: `Book ${this.title} has been created!`,
            showConfirmButton: false,
            timer: 6000,
          });
          const modal = document.querySelector('.modal');
          const sidebar = document.querySelector('.sidebar');
          modal.classList.toggle('is-active');
          sidebar.classList.toggle('show-sidebar');
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
};
</script>
<style scoped>
   .none{
    display: none !important;
  }
  .sidebar-button{
    border: none;
    cursor: pointer;
  }
  .sidebar {
    top: 0;
    left: 0;
    bottom: 0;
    width: 300px;
    min-height: 900px;
    height: 100vh;
    background: #ffffff;
    text-align: center;
    padding: 50px 0;
    position: fixed;
    transition: 1s;
    z-index: 2;
    margin-left: -300px;
    box-shadow: none;
  }
  .show-sidebar {
    margin-left: 0px;
    box-shadow: 3px 2px 20px #999999;
    transition: 1s;
  }
  .sidebar .menu-button {
    text-align: right;
    position: absolute;
    top: 20px;
    left: 240px;
  }
  .menu-button button {
    background: none;
  }
  .sidebar .profile {
    margin: 10px 0;
  }
  .profile img {
    width: 150px;
    height: 150px;
    border-radius: 50%;
  }
  .profile h4 {
    font-size: 20px;
    font-weight: bold;
    margin-top: 1em;
  }
  .sidebar .sidebar-menu {
    text-align: left;
    margin-top: 20px;
  }
  .sidebar-menu{
    padding: 0 30px;
  }
  .sidebar-menu p{
    margin: 1em 0;
  }
  .sidebar-menu p, .menu-list {
    color: #000000 !important;
    cursor: pointer;
    font-size: 1em;
    display: flex;
    flex-direction: column;
    transition: 0.5s;
  }
  .sidebar-menu p:hover, .menu-list {
    color: #424242;
    transition: 0.5s;
  }
  .search-box {
    width: 100%;
    flex-direction: row;
    padding: 10px 20px;
    border-radius: 20px;
    border: 1px solid #ced4da;
    display: none;
  }
  .search-box input{
    width: 100%;
    margin-left: 10px;
    border: none;
    outline: none;
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

  @media (max-width: 450px) {
    .search-box {
      display: flex;
    }
  }
</style>
