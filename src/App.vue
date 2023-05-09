<script setup>

import { ref, computed, onMounted } from 'vue';
import BlogPost from './components/BlogPost.vue';
import PaaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';


const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(true);

const favorito = ref("");    
const cambiarFavorito = (title) => {
  favorito.value = title
}

const next = () => { 
    inicio.value = inicio.value + postXpage;
    fin.value = fin.value + postXpage;
}

const prev = () => { 
    inicio.value += - postXpage;
    fin.value += - postXpage;
}

const maxLength = computed(() => posts.value.length)

// onMounted( async () => {
//     // loading.value = true;
//     try {
//        const res = await fetch('https://jsonplaceholder.typicode.com/posts');
//        posts.value = await res.json();
//     } catch (error) {
//         console.log(error)
//     } finally {
//         setTimeout(() => { // esto es opcional, solo para demostrar el spinner
//             loading.value = false;
//         }, 1500);
        
//     }
// });

// La mejor opcion de las 3
const fetchData = async() => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts');
    posts.value = await res.json();
  } catch (error) {
      console.log(error)
  } finally {
    setTimeout(() => { // esto es opcional, solo para demostrar el spinner
      loading.value = false;
    }, 1500);
  }
}
fetchData();

// fetch('https://jsonplaceholder.typicode.com/posts')
//       .then( res => res.json())
//       .then(data => posts.value = data)
//       .finally(() => loading.value = false)

</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <h1>App</h1>
    <h2>Mi Post Favorito: {{ favorito }}</h2>

    <PaaginatePost @next="next" @prev="prev" :inicio="inicio" :fin="fin" :maxLength="maxLength" class="mb-2" />
   
    <BlogPost 
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :title="post.title"
      :id="post.id"
      :body="post.body"
      @cambiarFavoritoNombre="cambiarFavorito"
      class="mb-2"
    />
    
  </div>
  
</template>

<style>



</style>