<template>
<div class="home">
  <br><br><br><br><br><br>
  <div v-if="photo!=null">
    <img :src="photo.path" />
    <div class="photoInfo">
      <p class="photoTitle">{{photo.title}}</p>
      <p class="photoName">{{photo.user.firstName}} {{photo.user.lastName}}</p>
    </div>
    <p class="photoDate">{{formatDate(photo.created)}}</p>
    <p class="photoDescription">{{photo.description}}</p>
  </div>
  <p v-if="error">{{error}}</p>
</div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';

export default {
  name: 'Home',
  components: {
  },
  data() {
    return {
      photo: null,
      error: '',
    }
  },
  created() {
  this.getPhoto();
  },
  methods: {
    async getPhoto() {
      try {
        let response = await axios.get("/api/photos/" + this.$route.params.id);
        this.photo = response.data;
      } catch (error) {
        this.error = error.response.data.message;
      }
    },
    formatDate(date) {
      if (moment(date).diff(Date.now(), 'days') < 15)
        return moment(date).fromNow();
      else
        return moment(date).format('d MMMM YYYY');
    },
  }
}
</script>
