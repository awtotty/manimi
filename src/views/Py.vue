<template>
  <div class="container">
    <v-btn
    @click="getVid(vidInfo)">
      Get Vid
    </v-btn>

    {{ res }}
  </div>
</template>
    

<script>
import axios from 'axios';

export default {
  data: () => ({
    vidInfo: { name: "hi" }, 
    res: null, 
  }), 

  methods: {
    getBooks() {
      const path = 'http://localhost:5000/books';
      axios.get(path)
        .then((res) => {
          this.books = res.data.books;
        })
        .catch((error) => {
          console.error(error);
        });
    },

    getVid(payload) {
      const path = 'http://localhost:5000/manim';
      axios.post(path, payload)
        .then( (res) => {
          this.res = res.data; 
          console.log("Server echo:\n" + res.data);
        })
        .catch((error) => {
          console.error(error);
        });
    },

  },

};
</script>

</script>