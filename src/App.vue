<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Anime from './components/Anime.vue'
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
  <el-row justify="center" class="custom-input">

    <el-col>
      <!-- <el-input v-model="input" placeholder="Please Input" seize="large" @keyup.enter="search()" :style="{
        width: '500px', height: '48px', borderRadius: '20x'
      }" :prefix-icon="Search"/>
      
      <el-button :icon="Search" @click="search()" :style="{height:'48px'}"></el-button> -->
    </el-col>

    <el-input v-model="input" placeholder="Type something" class="custom-input" :style="{
      width: '600px', height: '48px', borderRadius: '24x'
    }" @keyup.enter="search()">
      <template #prefix>
        <el-icon class="el-input__icon" @click="search()" style="cursor: pointer">
          <Search />
        </el-icon>
      </template>
    </el-input>
  </el-row>

  <el-row justify="center">
    <el-col span="6" v-for=" anime in animes">
      <Anime :name="anime.animeTitle" :img-url="anime.imageUrl" :episodes=anime.episodes />
      <!-- {{anime.animeTitle}}{{anime.imageUrl}} -->
    </el-col>
  </el-row>
</template>

<style scoped>
.custom-input {
  border-radius: 24px;

}

.custom-input>>>.el-input__wrapper {

  border-radius: 24px;
}
</style>
