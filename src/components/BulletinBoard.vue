<template>
    <div>
    <div class="form-container">
    <form v-if="!editingBulletin" @submit.prevent="addBulletin">
      
    <h1>佈告欄</h1>
      <label class="bulletin-text">
        標題:
        <input v-model="newBulletin.title" type="text" required />
      </label>
      <br />
      <label class="bulletin-text">
        內文:
        <textarea v-model="newBulletin.content" required></textarea>
      </label>
      <br />
      <button type="submit" class="edit-button" >新增佈告欄</button>
    </form>
  </div>

    <ul>
      <li v-for="bulletin in bulletins" :key="bulletin.id">
        <div v-if="bulletin === editingBulletin">
          <form @submit.prevent="updateBulletin">
            <label>
              標題:
              <input v-model="editingBulletin.title" type="text" required />
            </label>
            <br />
            <label>
              內文:
              <textarea v-model="editingBulletin.content" required></textarea>
            </label>
            <br />
            <button type="submit"  class="edit-button" >更新</button>
            <button type="button" @click="cancelEdit"  class="del-button"  >取消</button>
          </form>
        </div>
        <div v-else>
          <h2>主旨 : {{ bulletin.title }}</h2>
          <p>內文 : {{ bulletin.content }}</p>
          <button @click="editBulletin(bulletin)" class="edit-button" >編輯</button>
          <button @click="deleteBulletin(bulletin.id)" class="del-button" >刪除</button>
        </div>
      </li>
    </ul>
  </div>
</template>
  
<script>
import axios from 'axios'
export default {
  name: 'BulletinBoard',
  props: {
    msg: String
  },
  data() {
    return {
      bulletins: [],
      newBulletin: {
        title: '',
        content: '',
      },
      editingBulletin: null,
    }
  },
  methods: {
    async getBulletins() {
      const response = await axios.get('http://127.0.0.1:3000/announcements')
      this.bulletins = response.data
    },
    async addBulletin() {
      const response = await axios.post('http://127.0.0.1:3000/announcements', this.newBulletin)
      this.bulletins = (response.data)
      this.newBulletin = {
        title: '',
        content: '',
      }
    },
    async updateBulletin() {
      const response = await axios.put(
        `http://127.0.0.1:3000/announcements/${this.editingBulletin.id}`,
        this.editingBulletin,
      )
      this.bulletins = (response.data)
      this.editingBulletin = null
    },
    async deleteBulletin(id) {
      await axios.delete(`http://127.0.0.1:3000/announcements/${id}`)
      this.bulletins = this.bulletins.filter((bulletin) => bulletin.id !== id)
    },
    editBulletin(bulletin) {
      this.editingBulletin = bulletin
    },
    cancelEdit() {
      this.editingBulletin = null
    },
  },
  computed: {
    sortedBulletins() {
      return this.bulletins.slice().sort((a, b) => b.id - a.id)
    },
  },
  mounted() {
    this.getBulletins()
  },
}
</script>


<style scoped>
.edit-button {
    background-color: #4CAF50; /* Green */
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
  }

  .del-button {
    background-color: #ec0606; /* Green */
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
  }
  h2, p {
    text-align: left;
  }

  li {
    list-style: none;
    margin: 20px 0;
    border: 1px solid #ccc;
    padding: 20px;
  }
  .bulletin-text {
    text-align: left;
  }
  .form-container {
  display: flex;
  margin: 20px 0;
  border: 1px solid #ccc;
  padding: 20px;
}
</style>