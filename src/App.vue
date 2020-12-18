<template>
  <div id="app">
    <div>
      <b-navbar toggleable="lg" type="light" variant="light" fixed="top">
        <b-navbar-brand href="#">
          <img src="../src/assets/pinterest.png" />
        </b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav size="lg">
            <a href="#"><button class="btn btn-light me-2 rounded-pill" type="button">Home</button></a>
            <a href="#"><button class="btn btn-light me-2 rounded-pill" type="button">Following</button></a>
          </b-navbar-nav>
          <b-form-input size="lg" class="me-auto mb-2 mb-lg-0 rounded-pill" placeholder="Search" 
            v-model="topic" v-on:keyup.enter="searchUnsplash(topic)"></b-form-input>
          <!-- Right aligned nav items -->
          
          <b-navbar-nav size="lg">
            <b-nav-item-dropdown class="btn btn-light rounded-pill">
              <!-- Using 'button-content' slot -->
              <template #button-content>
                <b-icon icon="globe2" aria-hidden="true" ></b-icon>
              </template>
              <b-dropdown-item href="#">EN</b-dropdown-item>
              <b-dropdown-item href="#">DE</b-dropdown-item>
              <b-dropdown-item href="#">JP</b-dropdown-item>
              <b-dropdown-item href="#">CH</b-dropdown-item>
            </b-nav-item-dropdown>
            <b-nav-item-dropdown right class="btn btn-light rounded-pill">
              <!-- Using 'button-content' slot -->
              <template #button-content>
                User
              </template>
              <b-dropdown-item href="#">Profile</b-dropdown-item>
              <b-dropdown-item href="#">Sign Out</b-dropdown-item>
            </b-nav-item-dropdown>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>

    <div class="container">
      <stack :column-min-width="300"
              :gutter-width="15"
              :gutter-height="15"
              monitor-images-loaded>
        <stack-item v-for="(image, i) in images" :key="i" style="transition: transform 300ms">
          <img :src="image.urls.small" :alt="image.alt_description" >
        </stack-item>
      </stack>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import {Stack, StackItem} from 'vue-stack-grid';

export default {
  name: 'app',
  components: {
    Stack,
    StackItem,
  },
  data: () => ({
    images: [],
    topic: ""
  }),
  methods: {
    searchUnsplash(topic){
      this.images = []
      axios.get(`https://api.unsplash.com/search/photos?query=${topic}&per_page=30`,{
        headers: {
          Authorization: "Client-ID 3O9HxliWOc8WDgDcUSfzsxUtRdrfEjZIMOFMtKwYTDc",
          "Accept-Version":"v1"
        }
      }).then(response => {
        this.images = response.data.results;
      }).catch(() => {
        this.images = [];
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
}
.container {
  margin-top: 100px;
  width: 80vw;
}
.button-wrapper{
  display: flex;
  justify-content: center;
  margin-bottom: 25px;

}
.btn{
  font-size: 18px;
  background: #42b983;
  color: #fff;
  padding: 10px 20px;
}
img{
  width: 100%;
  height: auto;
  border-radius: 12px;
}
</style>
