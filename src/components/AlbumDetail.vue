<template>
    <div class="album-detail-section">
      <h2>Detail Album</h2>
      <div class="album">
        <h2>Pilih Album:</h2>
        <select v-model="selectedAlbum" @change="fetchAlbumDetail">
          <option v-for="album in albums" :key="album.id" :value="album.id">{{ album.title }}</option>
        </select>
      </div>
      <div v-if="album" class="album-detail">
        <h3>{{ album.title }}</h3>
        <div class="photo-cards">
          <div v-for="photo in photos" :key="photo.id" class="photo-card">
            <img :src="photo.thumbnailUrl" :alt="photo.title" @click="showPhoto(photo.url)" />
            <p>{{ photo.title }}</p>
          </div>
        </div>
      </div>
      <div v-else>
        <p>Tidak Ada Foto</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { useRoute } from 'vue-router'
  
  const album = ref(null)
  const photos = ref([])
  const selectedAlbum = ref(null)
  const albums = ref([])
  
  const route = useRoute()
  
  const fetchAlbums = async () => {
    try {
      const response = await fetch(`https://jsonplaceholder.typicode.com/albums`)
      albums.value = await response.json()
    } catch (error) {
      console.error('Error fetching albums:', error)
    }
  }
  
  const fetchAlbumDetail = async () => {
    try {
      const albumResponse = await fetch(`https://jsonplaceholder.typicode.com/albums/${selectedAlbum.value}`)
      album.value = await albumResponse.json()
      const photosResponse = await fetch(`https://jsonplaceholder.typicode.com/photos?albumId=${selectedAlbum.value}`)
      photos.value = await photosResponse.json()
    } catch (error) {
      console.error('Error fetching album details:', error)
    }
  }
  
  const showPhoto = (url) => {
    window.open(url, '_blank')
  }
  
  onMounted(async () => {
    await fetchAlbums()
    selectedAlbum.value = route.params.id || albums.value[0]?.id
    if (selectedAlbum.value) {
      fetchAlbumDetail()
    }
  })
  </script>
  
  <style scoped>
  .album-detail-section {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    background-image: url('https://images.pexels.com/photos/264940/pexels-photo-264940.jpeg');
    background-size: cover;
    background-position: center;
    min-height: 100vh;
  }
  
  h2 {
    margin-bottom: 1.5rem;
    color: white;
  }
  
  .album-detail {
    text-align: center;
    font-family: 'Times New Roman, sans-serif';
  }
  
  .photo-cards {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 1rem;
  }
  
  .photo-card {
    width: 150px;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(170, 27, 27, 0.1);
    padding: 1rem;
    text-align: left;
  }
  
  .photo-card img {
    width: 100%;
    border-radius: 4px;
    margin-bottom: 0.5rem;
    cursor: pointer;
    transition: border-color 0.3s;
  }
  
  .photo-card img:hover {
    border: 2px solid #007bff;
  }
  
  .photo-card p {
    color: #333;
  }
  
  .album select{
    padding: 10px 20px;
    margin-bottom: 20px;
    border: 2.5px solid #3add08;
    border-radius: 10px;
    font-size: 18px;
    font-weight: 500;
    text-align: center;
  }
  
  .album h2{
    color: #16d12f;
    font-weight: 700;
    text-align: center;
    margin-bottom: 20px;
  }
  
  .album h5{
    color: #29e90f;
    font-weight: 400;
    text-align: center;
    margin-bottom: 20px;
  }
  
  .albumt{
    border-collapse: collapse;
    border-spacing: 10 10px;
    padding:20px;
  }
  </style>
  