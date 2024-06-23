<template>
    <section class="post-section">
      <h2>Postingan Pengguna</h2>
      <div class="select-user">
        <label for="user-select">Pilih Pengguna:</label>
        <select id="user-select" v-model="selectedUser" @change="fetchPosts" class="select-box">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div v-if="posts.length > 0" class="user-posts">
        <h3>Postingan Pengguna: {{ selectedUserName }}</h3>
        <div class="post-cards">
          <div v-for="post in posts" :key="post.id" class="post-card">
            <h4>{{ post.title }}</h4>
            <p>{{ post.body }}</p>
          </div>
        </div>
      </div>
    </section>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  
  const selectedUser = ref(null)
  const users = ref([])
  const posts = ref([])
  const selectedUserName = ref('')
  
  const fetchUsers = async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/users')
      users.value = await response.json()
    } catch (error) {
      console.error('Error fetching users:', error)
    }
  }
  
  const fetchPosts = async () => {
    if (!selectedUser.value) return
    try {
      const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`)
      posts.value = await response.json()
      selectedUserName.value = users.value.find(user => user.id === selectedUser.value)?.name || ''
    } catch (error) {
      console.error('Error fetching posts:', error)
    }
  }
  
  onMounted(fetchUsers)
  </script>
  
  <style scoped>
  .post-section {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    background: linear-gradient(to right, #37ad09, #5482e6);
    min-height: 100vh;
    color: #eaecea;
    font-family: 'Times New Roman, sans-serif';
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
  }
  
  h2 {
    margin-bottom: 1.5rem;
    font-size: 2rem;
  }
  
  .select-user {
    margin-bottom: 1.5rem;
  }
  
  .select-user label {
    margin-right: 1rem;
  }
  
  .select-box {
    padding: 8px 12px;
    font-size: 16px;
    border: 1px solid #cdfa05;
    border-radius: 5px;
    background-color: #fff;
    transition: border-color 0.3s, background-color 0.3s;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .select-box:focus {
    outline: none;
    border-color: #d9ee20;
    background-color: white;
  }
  
  .user-posts {
    text-align: center;
  }
  
  .post-cards {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 1rem;
  }
  
  .post-card {
    width: 300px;
    background-color: rgb(230, 225, 225);
    border-radius: 12px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    padding: 1.5rem;
    text-align: left;
    transition: transform 0.2s;
  }
  
  .post-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }
  
  .post-card h4 {
    margin-bottom: 0.5rem;
    font-size: 1.25rem;
    color: #333;
  }
  
  .post-card p {
    color: #666;
    line-height: 1.6;
  }
  </style>
  