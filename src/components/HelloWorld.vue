<script setup lang="ts">
import { ref, watch, toRefs } from 'vue'
import get from 'axios'

const pops = defineProps(['imgUrl', 'name', 'episodes'])
const dialogTableVisible = ref(false)
const data = ref()
function showEp(eps: any) {
  // console.log(eps)
  dialogTableVisible.value = true
  data.value = pops.episodes
  console.log(data.value)
}

</script>

<template>
  <el-card :body-style="{ padding: '0px' }">
    <img :src="pops.imgUrl" class="image" />
    <div style="padding: 14px">
      <span>{{ name }}</span>
      <div class="bottom">
        <!-- <time class="time">{{ currentDate }}</time> -->
        <el-button text class="button" @click='showEp'>展开</el-button>
      </div>
    </div>
  </el-card>

  <el-dialog v-model="dialogTableVisible">
    <el-table :data="data">
      <el-table-column property="episodeTitle" label="集数标题" width="400" />
    </el-table>
  </el-dialog>
</template>


