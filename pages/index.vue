<template>
    <main class="p-4">
  
      <AddPost v-if="ShowModal" @createPost="addPost" />
  
      <section class="flex flex-col gap-2">
        <article class="flex gap-2">
          <!-- Кнопка для смены порядка сортировки -->
          <button @click="toggleOrder" 
                  :class="['py-2 px-4 border rounded', orderById ? 'bg-green-400 hover:bg-green-500' : 'bg-gray-200 hover:bg-gray-300']">
            Change Order
          </button>
          <!-- Кнопка для добавления поста -->
          <button @click="ShowModal = !ShowModal" 
                  class="py-2 px-4 border rounded bg-blue-400 text-white hover:bg-blue-500">
            Add Post
          </button>
        </article>
  
        <!-- Список постов -->
         <section>
                <Post v-for="item in sortedPosts" :key="item.id" 
                        :info="item">
                <!-- <p class="text-lg font-bold">{{ item.id }}</p>
                <h3 class="text-xl">{{ item.title }}</h3>
                <p>{{ item.body }}</p> -->
                </Post>
        </section>
  
        <!-- Пагинация -->
        <article class="flex gap-2 items-center mt-4">
          <p class="text-lg">Page: {{ currentPage }}</p>
          <button @click="previousPage" 
                  :disabled="currentPage <= 1" 
                  class="py-2 px-4 border rounded bg-gray-200 hover:bg-gray-300 transition">
            Previous
          </button>
          <button @click="nextPage" 
                  :disabled="currentPage >= maxPages" 
                  class="py-2 px-4 border rounded bg-gray-200 hover:bg-gray-300 transition">
            Next
          </button>
        </article>
      </section>
    </main>
  </template>
  

<script lang="ts" setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue';

// Ссылки на состояние
const posts = ref([]);
const currentPage = ref(1);
const orderById = ref(true);
const ShowModal = ref(false);
// Получение и установка постов при монтировании
onMounted(async () => {
  posts.value = await getPosts();
  maxPages.value = Math.ceil(posts.value.length / 10);
  JSON.parse(localStorage.getItem("OurPosts")).forEach(item => {
    posts.value.push(item);
  })
});

// Функция для получения постов
async function getPosts() {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts');
  return await response.json();
}

// Вычисляемые данные для сортировки и отображения постов на текущей странице
const sortedPosts = computed(() => {
  const items = getTenPages(posts.value, currentPage.value);
  return items.sort((a, b) => (orderById.value ? a.id - b.id : b.id - a.id));
});
const maxPages = computed(() => {
    return Math.ceil(posts.value.length / 10);
});
// Функция для переключения порядка сортировки
function toggleOrder() {
  orderById.value = !orderById.value;
}

// Функции для навигации по страницам
function nextPage() {
  if (currentPage.value < maxPages.value) currentPage.value++;
}

function previousPage() {
  if (currentPage.value > 1) currentPage.value--;
}

// Функция для получения 10 постов на текущей странице
function getTenPages(posts, currentPage) {
  const startIndex = (currentPage - 1) * 10;
  return posts.slice(startIndex, startIndex + 10);
}

function addPost(post) {
    const newPost = { id: posts.value.length + 1, title: post.title, body: post.text };
  posts.value.push(newPost);
    localStorage.setItem("OurPosts", JSON.stringify(posts.value.slice(100, posts.value.length)));
    console.log(posts.value)
  ShowModal.value = false;
}
</script>


