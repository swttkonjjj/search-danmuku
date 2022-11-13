<script setup lang="ts">
import { ref, watch, toRefs } from 'vue'
import X2JS from 'X2JS'
import get from 'axios'
import { ElLoading } from 'element-plus'

const currentRow = ref()
const handleCurrentChange = (val: Object | undefined) => {
  currentRow.value = val
}

const pops = defineProps(['imgUrl', 'name', 'episodes'])

const download = () => {
  console.log(currentRow.value.episodeId)
  get('https://api.dandanplay.net/api/v2/comment/' + currentRow.value.episodeId, {
    params: {
      withRelated: true,
      chConvert: 1
    }}).then((responmse) => {
    console.log(responmse)
    let xml = '<?xml version="1.0"?><i>'
    xml += '<chatserver>chat.bilibili.com</chatserver><maxlimit>8000</maxlimit><max_count>8000</max_count><chatid>10000</chatid>'
    console.log(xml)
    for(let i in responmse.data.comments){
      const comment = responmse.data.comments[i]
      const m = comment.m.replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/&/g,'&amp;').replace(/"/g,'&quot;')
      // console.log(comment.p)
      // const p:string = comment.p.split(',').splice(3,0,'25').splice(4,0,['0','0']).push('0').join()
      const arg = comment.p.split(',')
      console.log(arg)
      const p = arg.slice(0,2) + ',25,' + arg[2] + ',0,0,' + arg[3] +',0'
      xml += '<d p="'+ p +'">' + m + '</d>'
    }
    xml += '</i>'
    var element = document.createElement('a');

    element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(xml));
    element.setAttribute('download', currentRow.value.episodeTitle + '.xml');

    element.style.display = 'none';
    document.body.appendChild(element);

    element.click();

    document.body.removeChild(element);
  })
}

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
    <el-table :data="data" highlight-current-row @current-change="handleCurrentChange">
      <el-table-column property="episodeTitle" label="集数标题" width="400" />
      <el-table-column v-if="false" property="episodeId" label="集数标题" width="400" />
    </el-table>
    <el-button @click="download">下载选中集数的弹幕</el-button>
  </el-dialog>
</template>


