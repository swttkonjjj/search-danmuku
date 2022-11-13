<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'
import { ref } from 'vue'
import { Search } from '@element-plus/icons-vue'
import get from 'axios'

const input = ref('')
let anime = ref('')
const animes = ref()
const api = 'https://api.dandanplay.net/api/v2/search/episodes'
const bgm = 'https://api.dandanplay.net/api/v2/bangumi/'
const url = "https://shadow.elemecdn.com/app/element/hamburger.9cf7b091-55e9-11e9-a976-7f4d0b07eef6.png"
function search() {
  get(api, {
    params: {
      anime: input.value
    }
  }).then((response) => {
    animes.value = response.data.animes
    console.log(response)
    for (let i in animes.value) {
      get(bgm + animes.value[i].animeId).then(
        (response) => {
          animes.value[i].imageUrl = response.data.bangumi.imageUrl
          // animes.value[i].episodes = response.data.bangumi.episodes
          // console.log(response)
        }
      )
    }
  })
}

</script>

<template>

  <el-container>
    <el-input v-model="input" placeholder="Please Input" />
    <el-button :icon="Search" @click="search()">Search</el-button>
  </el-container>
  
  <el-row>
<el-col span="8" v-for=" anime in animes">
  <HelloWorld :name="anime.animeTitle" :img-url="anime.imageUrl" :episodes= anime.episodes />
  <!-- {{anime.animeTitle}}{{anime.imageUrl}} -->
</el-col>
</el-row>

</template>

<style scoped>

</style>
