<template>
  <div class="row">
    <div class="leftcolumn">
      <div class="card" v-for="item in items" :key="item.id">
        <h2 id='title'>{{item.title}}</h2>
        <div class="fakeimg">
          <img v-if="item.path" :src="item.path" />
        </div>
        <p>{{item.description}}</p>
      </div>
    </div>
    <div class="rightcolumn">
      <div class="card">
        <div id="thisAboutMe">
          <h2 id="aboutme">About Me</h2>
          <div class="fakeimg" style="height:100px;"><img src='/images/forannyhavn.jpg'></div>
          <p>I'm a 22 year old adventure seeker in CS 260!</p>
        </div>
      </div>
      <div class="card">
        <div id="thisAboutMe">
          <h3>Follow Me</h3>
          <p><a href="https://github.com/raywray/creativeproject4.git">@github</a></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: 'Home',
  data() {
    return {
      items: [],
    }
  },
  created() {
    this.getItems();
  },
  methods: {
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>

<style scoped>

body {
  font-family: Arial;
  padding: 20px;
  background: white;
  color: #525252;
}

#title {
  font-size: 30px;
}

#aboutme {
  font-size: 20px;
}

#thisAboutMe {
  background-color: #e3e3e3;
  padding: 10px;
}

/* Create two unequal columns that floats next to each other */
/* Left column */
.leftcolumn {
  float: left;
  width: 70%;
}

/* Right column */
.rightcolumn {
  float: right;
  width: 20%;
  padding-left: 20px;

}

/* Fake image */
.fakeimg {
  width: auto;
  align-items: center;
  height: 400px;
}

img {
  height: 100%;
  width: auto;
}

/* Add a card effect for articles */
.card {
  background-color: white;
  padding: 20px;
  margin-top: 20px;
  text-align: center;
}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

/* Responsive layout - when the screen is less than 800px wide, make the two columns stack on top of each other instead of next to each other */
@media screen and (max-width: 800px) {
  .leftcolumn, .rightcolumn {
    width: 100%;
    padding: 0;
  }
}

</style>
