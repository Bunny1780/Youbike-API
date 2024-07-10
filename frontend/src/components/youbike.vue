<script setup>
import {ref} from "vue";
const api = "https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json"
const query = ref('');
const sites = ref([]);
const filterSites = ref([])

const handlerSubmit = async () => {
    try {
        if(query.value.trim() !== "") {
            const response = await fetch(api)
            const posts = await response.json()
            sites.value = posts
            filterSites.value = sites.value.filter(site => site.ar.includes(query.value.trim()))
        }
    } catch (error) {
        console.log(`Error fetching data:, ${error}`);
    }
}
</script>

<template>
    <main class="container mx-auto p-4 max-w-lg h-1/2">
        <form @submit.prevent="handlerSubmit" class="mb-4">
            <div>
                <label class="block text-lg font-medium">YouBike 即時資料查詢</label>
                <small class="text-gray-500">資料來源：<a class="text-gray-500 underline" href="https://data.gov.tw/dataset/137993" target="_blank">政府資料開放平台</a></small>
            </div>
            <div class="flex mt-2">
                <input v-model.trim="query" type="text" class="flex-grow p-2 border border-gray-300 rounded-md text-lg" placeholder="可輸入關鍵字搜尋路名，例如：八德路">
                <button class="ml-2 p-2 bg-green-500 text-white rounded-md text-lg hover:bg-green-400 transition delay-100 duration-100 ease-in-out">搜尋</button>
            </div>
        </form>

        <ul class="siteList space-y-2 max-h-462 overflow-y-auto">
            <li class="p-4 border border-gray-300 rounded-md bg-white" v-for="filterSite in filterSites">
                <i class="fas fa-bicycle text-xl"></i>
                <span class="text-lg font-semibold">{{filterSite.sna.replace("YouBike2.0_", "")}} ({{filterSite.available_rent_bikes}})</span><br>
                <small class="text-gray-500">{{filterSite.ar}}</small>
            </li>
        </ul>
        <hr class="mt-4">
    </main>
</template>

<style scoped>

</style>
