<template>
<div class="page-content">
  <div class="admin">
    <div id="newpost"><h1>New Post</h1></div>
    <div class='postbox'>
      <div class="heading">
        <div class="circle">+</div>
        <h2>Write New Post</h2>
      </div>
      <div class="add">
        <div class="form">
          <input id="long" v-model="title" placeholder="Add Title">
          <textarea v-model="description" placeholder="Write Post"></textarea>
          <input type="file" name="photo" @change="fileChanged">
          <button @click="upload">Upload</button>
        </div>
        <div class="upload" v-if="addItem">
          <h2>{{addItem.title}}</h2>
          <h3>{{addItem.description}}</h3>
          <img :src="addItem.path" />
        </div>
      </div>
      <div class="heading">
        <div class="circle">-</div>
        <h2>Edit/Delete a Post</h2>
      </div>
      <div class="edit">
        <div class="form">
          <input v-model="findTitle" placeholder="Search">
          <div class="suggestions" v-if="suggestions.length > 0">
            <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
            </div>
          </div>
        </div>
        <div class="upload" v-if="findItem">
          <input v-model="findItem.title">
          <input v-model="findItem.description">
          <p></p>
          <img :src="findItem.path" />
        </div>
        <div class="actions" v-if="findItem">
          <button @click="deleteItem(findItem)">Delete</button>
          <button @click="editItem(findItem)">Edit</button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Admin',
  data() {
    return {
      title: "",
      file: null,
      addItem: null,
      description: "",
      items: [],
      findTitle: '',
      findItem: null,
    }
  },
  computed: {
    suggestions() {
      let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    }
  },
  created() {
    this.getItems();
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
    },
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          description: this.findItem.description,
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async upload() {
      try {
        if (this.file)  {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          title: this.title,
          path: r1.data.path,
          description: this.description,
        });
        this.addItem = r2.data;
        }
        else {
        let r2 = await axios.post('/api/items', {
          title: this.title,
          path: null,
          description: this.description,
        });
        this.addItem = r2.data;
        }
      } catch (error) {
        console.log(error);
      }
    },
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
.image h2 {
  font-style: italic;
  font-size: 1em;
}
.image h2 {
  font-size: .5em;
}

.page-content {
  align-items: center;

  width: auto;
}

.admin {
  margin-left: 33.3%;
  width: 33.3%;
  height: auto;

}

#newpost {
  text-align: center;
  height: auto;
}

.postbox {
  width: auto;
}

#long {
  padding-right: 200px;
  padding-bottom: 30px;
}

textarea {
  resize: none;
  height: 400px;
  width: 400px;
}

h1 {
  font-size: 30px;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

.circle {
  width: 18px;
  height: 18px;
  padding: 5px;
  background: white;
  border: 1px dotted grey;
  color: dark grey;
  font-size: 15px;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
  height: auto;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}

h2 {
  font-size: 14px;
}
</style>
