<script setup>
import ArticlePreview from '../components/ArticlePreview.vue';
import SectionTitle from '../components/SectionTitle.vue';
import MailingList from '../components/MailingList.vue';
import formatDate from '@/utils/formatDate';
import axios from 'axios';
import { onMounted, ref } from 'vue';
import LoadingSpinner from '@/components/icons/LoadingSpinner.vue';
import bgImage from '../assets/bg.jpg'

const articles = ref({
    latest: [],
    popular: []
});
const isLoading = ref(true);

async function fetchLatestArticles(){
    try {
        const response = await axios.get('https://deliberate-collie-birdiepoop-197e5571.koyeb.app/api/posts/list/latest'); 
        articles.value.latest = response.data.posts;
    } catch (error) {
        console.error('Error fetching latest articles', error);
    }
}

async function fetchPopularArticles(){
    try {
        const response = await axios.get('https://deliberate-collie-birdiepoop-197e5571.koyeb.app/api/posts/list/popular');
        articles.value.popular = response.data.posts;
    } catch (error) {
        console.error('Error fetching latest articles', error);
    }
}
onMounted(async () => {
    try {
        await fetchLatestArticles();
        await fetchPopularArticles();
    } catch (error) {
        console.error('Error fetching data');
    } finally {
        isLoading.value = false;
    }
    
})

</script>

<template>
    <header class=" h-[30rem] bg-cover bg-gray-50" :style="{ backgroundImage: `url(${bgImage})`, backgroundPosition: 'center 14%'}">
        <div class=" container mx-auto px-4 flex justify-end h-full py-6 items-center">
            <div class="bg-white bg-opacity-50 rounded-md p-10 max-w-xl">
                <p class=" text-sky-500 uppercase tracking-wider">bird wisdom</p>
                <h2 class=" text-3xl xl:text-4xl font-bold mt-4 text-gray-800 capitalize">Here to fulfill your daily
                    bird news needs</h2>
                <p class=" text-gray-700 drop-shadow-sm mt-4 text-lg">Welcome to a unique world of knowledge, where a wise bird shares insights on everything under the sun!
                                                             From the latest news to intriguing facts, this is your go-to place for diverse information, delivered with 
                                                             a touch of feathered wisdom. Stay curious and explore a variety of topics, all narrated by your friendly avian guide.</p>
            </div>
        </div>
    </header>
    <section class=" container mx-auto px-4 py-16">
        <SectionTitle>Latest Articles</SectionTitle>
        <div v-if="isLoading" class="grid grid-cols-1 gap-8 md:grid-cols-2 mt-6 xl:grid-cols-3">
            <LoadingSpinner></LoadingSpinner>
        </div>
        <div v-else class="grid grid-cols-1 gap-8 md:grid-cols-2 mt-6 xl:grid-cols-3">
            <ArticlePreview v-for="article in articles.latest" :key="article.id" :id="article.id" :title="article.title" :text="article.text" :author="article.author.username"
                :created-at="formatDate(article.createdAt)"></ArticlePreview>
        </div>
    </section>
    <MailingList></MailingList>
    <section class=" container mx-auto px-4 py-10 lg:py-16">
        <SectionTitle>Most Popular</SectionTitle>
        <div v-if="isLoading" class="grid grid-cols-1 gap-8 md:grid-cols-2 mt-6 xl:grid-cols-3">
            <LoadingSpinner></LoadingSpinner>
        </div>
        <div v-else class="grid grid-cols-1 gap-8 md:grid-cols-2 mt-6 xl:grid-cols-3">
            <ArticlePreview v-for="article in articles.popular" :key="article.id" :id="article.id" :title="article.title" :text="article.text" :author="article.author.username"
            :created-at="formatDate(article.createdAt)"></ArticlePreview>
        </div>
    </section>
</template>