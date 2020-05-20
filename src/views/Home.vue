<template>
  <div>
    <nav class="column is-gapless custom-navbar" v-if="!isLogin">
      <div class="button-form">
        <router-link to='/login' class="button is-white" type="submit">Login</router-link>
        <router-link to='/register' class="button is-black signup">Sign Up</router-link>
      </div>
        <div class="column sort-book">
          <div class="dropdown categories">
            <div class="dropdown-trigger">
              <a class="button" aria-haspopup="true" aria-controls="dropdown-menu"
              v-on:click="active1">
                <span>All Categories</span>
                <span class="icon is-small">
                  <i class="fas fa-angle-down" aria-hidden="true"></i>
                </span>
              </a>
            </div>
            <div class="dropdown-menu" id="dropdown-menu" role="menu">
              <div class="dropdown-content">
                <a href="#" class="dropdown-item">
                  Novel
                </a>
                <hr class="dropdown-divider">
                <a href="#" class="dropdown-item">
                  Komik
                </a>
              </div>
            </div>
          </div>
          <div class="dropdown time">
            <div class="dropdown-trigger">
              <a class="button" aria-haspopup="true" aria-controls="dropdown-menu"
              v-on:click="active2">
                <span>All Time</span>
                <span class="icon is-small">
                  <i class="fas fa-angle-down" aria-hidden="true"></i>
                </span>
              </a>
            </div>
            <div class="dropdown-menu" id="dropdown-menu" role="menu">
              <div class="dropdown-content">
                <a href="#" class="dropdown-item">
                  Terbaru
                </a>
                <hr class="dropdown-divider">
                <a href="#" class="dropdown-item">
                  Terlama
                </a>
              </div>
            </div>
          </div>
        </div>
        <div class="column search">
          <div class="search-box">
            <i class="fas fa-search"></i>
            <input
              class="form-control mr-sm-2"
              type="search"
              placeholder="Search..."
              aria-label="Search" />
          </div>
        </div>
        <div class="column navbar-brand">
          <router-link to="/">
          <img src="../assets/img/bookshelf.png" alt="logo" height="45px" width="45px">
          </router-link>
          <router-link to="/" class="back-home">Library</router-link>
        </div>
    </nav>
    <nav class="column is-gapless custom-navbar" v-else>
      <div>
        <sidebar/>
          <div class="toggle-menu" @click="sidebarShow">
            <img src="../assets/img/menu.png" alt="toggle-menu">
          </div>
      </div>
      <div class="column sort-book">
        <div class="dropdown categories">
          <div class="dropdown-trigger">
            <a class="button" aria-haspopup="true" aria-controls="dropdown-menu"
            v-on:click="active1">
              <span>All Categories</span>
              <span class="icon is-small">
                <i class="fas fa-angle-down" aria-hidden="true"></i>
              </span>
            </a>
          </div>
          <div class="dropdown-menu" id="dropdown-menu" role="menu">
            <div class="dropdown-content">
              <button class="dropdown-item" @click='sortByTitle()'>
                Sort By Title
              </button>
              <button href="#" class="dropdown-item" @click='sortByAuthor()'>
                Sort By Author
              </button>
            </div>
          </div>
        </div>
          <div class="dropdown time">
            <div class="dropdown-trigger">
              <a class="button" aria-haspopup="true" aria-controls="dropdown-menu"
              v-on:click="active2">
                <span>All Time</span>
                <span class="icon is-small">
                  <i class="fas fa-angle-down" aria-hidden="true"></i>
                </span>
              </a>
            </div>
            <div class="dropdown-menu" id="dropdown-menu" role="menu">
              <div class="dropdown-content">
                <button class="dropdown-item" @click="sortByTime1()">
                  Recently Updated
                </button>
                <button class="dropdown-item" @click="sortByTime2()">
                  Least Recently Updated
                </button>
              </div>
            </div>
          </div>
        </div>
        <div class="column search">
          <div class="search-box">
            <i class="fas fa-search"></i>
            <input
              class="form-control mr-sm-2"
              type="search"
              placeholder="Search..."
              aria-label="Search" v-model="search" @input="searchBook" />
          </div>
        </div>
        <div class="column navbar-brand">
          <router-link to="/">
          <img src="../assets/img/bookshelf.png" alt="logo" height="45px" width="45px">
          </router-link>
          <router-link to="/" class="back-home">Library</router-link>
        </div>
    </nav>
    <Carousel class="carousel"/>
    <div class="book-list">
      <h1>List Book</h1>
            <div class="list-book">
        <div class="cards" v-for="book in books.data" :key="book.data">
          <router-link :to="'detail/' + book.id">
          <Card :bookTitle="book.title" :bookAuthor="book.author" :image="book.image"/>
          </router-link>
        </div>
      </div>
    </div>
    <nav class="pagination is-centered" role="navigation" aria-label="pagination">
        <div class="pagination-list">
          <button class="pagination-previous" v-on:click="previous()">Previous</button>
          <button class="pagination-next" v-on:click="next()">Next page</button>
        </div>
    </nav>
    <br>
    <br>
    <br>
    <br>
  </div>
</template>

<script>
import axios from 'axios';
import Carousel from '../components/module/Carousel.vue';
import Card from '../components/module/Card.vue';
import Sidebar from '../components/module/Sidebar.vue';

export default {
  name: 'Home',
  components: {
    Carousel,
    Card,
    Sidebar,
  },
  data() {
    return {
      books: [],
      isLogin: false,
      url: 'http://localhost:8000/api/v1/book/?',
      count: 0,
    };
  },

  created() {
    this.items = JSON.parse(localStorage.getItem('items'));
    // console.log(this.items);
    if (this.items) {
      this.isLogin = this.items.isLogin;
    }
  },
  methods: {
    page() {
    },
    getAllBooks() {
      axios.get(this.url)
        .then((res) => {
          this.books = res.data;
          console.log(this.books);
        })
        .catch(() => {
        // console.log(err);
        });
    },
    sidebarShow() {
      const sidebar = document.querySelector('.sidebar');
      sidebar.classList.toggle('show-sidebar');
    },
    active1() {
      const dropdown1 = document.querySelector('.categories');
      dropdown1.classList.toggle('is-active');
    },
    active2() {
      const dropdown2 = document.querySelector('.time');
      dropdown2.classList.toggle('is-active');
    },
    sortByTime1() {
      this.url = 'http://localhost:8000/api/v1/book/?sort=createdAt&sort_type=desc';
      this.getAllBooks();
    },
    sortByTime2() {
      this.url = 'http://localhost:8000/api/v1/book/?sort=createdAt&sort_type=asc';
      this.getAllBooks();
    },
    sortByTitle() {
      this.url = 'http://localhost:8000/api/v1/book/?sort=title';
      this.getAllBooks();
    },
    sortByAuthor() {
      this.url = 'http://localhost:8000/api/v1/book/?sort=author';
      this.getAllBooks();
    },
    searchBook() {
      this.url = `http://localhost:8000/api/v1/book/?search=${this.search}`;
      this.getAllBooks();
    },
    next() {
      console.log(this.books.next_link);
      this.url = this.books.next_link;
      this.sort = '';
      this.getAllBooks();
      // this.count += 1;
    },
    previous() {
      // console.log(this.books.prev_link);
      this.url = this.books.prev_link;
      this.sort = '';
      this.getAllBooks();
    },
  },
  // computed: {
  //   jumlah() {
  //     return this.count * 1;
  //   },
  // },
  watch: {
    url() {
      // this.getAllBooks();
      this.url = this.books.next_link;
    },
  },
  mounted() {
    this.getAllBooks();
  },
};
</script>

<style scoped>
/* * {
  padding: 0;
  margin: 0;
} */
.pagination-next {
  justify-items: center;
}
.custom-navbar{
    max-width: 100%;
    background-color: #fff;
    align-items: center;
    box-shadow: 3px 2px 20px rgba(0, 0, 0, 0.2);
    margin: 0;
    padding: 5px;
    height: 60px;
}
.button {
  font-family: Airbnb Cereal App;
  font-size: 17px;
}

.button-form {
  margin-left: 30px;
}

.is-white{
  margin-left: 10px;
  padding: 10px;
  margin-right: 20px;
}

.is-black {
  padding: 15px;
}

.is-white:hover{
  transition: 0.5s;
  color: #fff;
  background: #000;
}

 .custom-navbar .toggle-menu{
    margin: 0 30px;
  }
   .toggle-menu{
    cursor: pointer;
  }
  .column{
    display: flex;
    flex-direction: row;
  }
  .sort-book{
    display: flex;
    justify-content: center;
  }
  .button span{
    font-size: 17px;
  }
   .button{
    border: none;
  }

.time {
  margin-left: 30px;
}

.search-box {
    width: 100%;
    display: flex;
    flex-direction: row;
    padding: 10px 20px;
    border-radius: 20px;
    border: 1px solid #ced4da;
}

.search-box input{
  width: 100%;
  margin-left: 10px;
  border: none;
  outline: none;
}

.navbar-brand {
  display: flex;
  align-items: center;
  justify-content:center;
}

.navbar-brand img{
  margin-right: 10px;
}

.navbar-brand .back-home{
  color: #000;
  font-family: Airbnb Cereal App Bold;
  font-size: 25px;
}

.carousel {
  margin-top: 50px;
}

/* List Book */
  .book-list h1 {
    font-size: 35px;
    font-weight: bold;
    margin-left: 50px;
  }
  .list-book {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
    margin-left: 100px;
    margin-right: 100px;
    margin-top: 20px;
  }
/* End of List Book */

.pagination {
  margin-top: 20px;
}

@media (max-width: 992px) {
  .categories {
    display: none;
  }
  .signup {
    display: none;
  }
  .custom-navbar {
    padding: 0px;
    margin: 0px;
  }
}
@media (max-width: 450px) {
  .sort-book {
    display: none;
  }
  .search {
    display: none;
  }
}

</style>
