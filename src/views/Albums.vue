<template>
  <div class="AlbumDetails-container">
    <h2 class="title">Choose Album:</h2>
    <select v-model="selectedAlbum" @change="fetchPhotos" class="select-album">
      <option disabled value="">Please select an album</option>
      <option v-for="album in albums" :key="album.id" :value="album.id">{{ album.title }}</option>
    </select>
    <h2 class="title">Photos in Album {{ selectedAlbum }}</h2>
    <table v-if="photos.length" class="photos-table">
      <thead>
        <tr>
          <th>Thumbnail</th>
          <th>Title</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="photo in photos" :key="photo.id">
          <td>
            <img :src="photo.thumbnailUrl" @click="showPhoto(photo.url)" class="thumbnail">
          </td>
          <td>{{ photo.title }}</td>
        </tr>
      </tbody>
    </table>
    <div v-else class="no-photos">
      <p>Photo not available</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import { useRouter } from 'vue-router';
import { useAlbumsStore } from '../stores/AlbumStore';

const router = useRouter();
const selectedAlbum = ref(null);
const albums = ref([]);
const photos = ref([]);

const albumsStore = useAlbumsStore();

const fetchAlbums = async () => {
try {
  await albumsStore.fetchAlbums();
  albums.value = albumsStore.albums;
} catch (error) {
  console.error('Error fetching albums:', error);
}
};

const fetchPhotos = async () => {
if (selectedAlbum.value) {
  try {
    await albumsStore.fetchPhotos(selectedAlbum.value);
    photos.value = albumsStore.photos;
  } catch (error) {
    console.error('Error fetching photos:', error);
  }
}
};

const showPhoto = (url) => {
window.open(url, '_blank');
};

onMounted(() => {
fetchAlbums();
});

watch(selectedAlbum, (newAlbum) => {
if (newAlbum) {
  fetchPhotos();
  router.push(`/albums/${newAlbum}`);
}
});
</script>

<style scoped>
.AlbumDetails-container {
display: flex;
flex-direction: column;
justify-content: center;
align-items: center;
background-color: #f8f8f8;
border-radius: 10px;
padding: 20px;
box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
margin: 20px auto;
max-width: 2000px;
}

.title {
color: #333;
text-align: center;
font-weight: bold;
font-size: 1.5rem;
margin-bottom: 15px;
}

.select-album {
margin: 0 auto 20px;
display: block;
padding: 10px 15px;
font-size: 1.5rem;
border-radius: 10px;
border: 1px solid #ccc;
background-color: #fff;
}

.photos-table {
width: 100%;
border-collapse: collapse;
margin-top: 20px;
}

.photos-table th, .photos-table td {
padding: 30px;
text-align: left;
}

.photos-table th {
background-color: #f1f1f1;
border-bottom: 2px solid #ddd;
}

.thumbnail {
width: 80px;
height: 80px;
object-fit: cover;
border: 1px solid #ccc;
border-radius: 5px;
cursor: pointer;
transition: border-color 0.3s;
}

.thumbnail:hover {
border-color: #007bff;
}

.no-photos {
text-align: center;
margin-top: 20px;
}

.no-photos p {
font-size: 1.2rem;
color: #777;
}
</style>
