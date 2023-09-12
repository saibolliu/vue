<template>
  <div class="docx-container">
    <vue-office-docx 
        :src="docx"
        @rendered="rendered"
    />
</div>  
</template>

<script>

import VueOfficeDocx from '@vue-office/docx'

import '@vue-office/docx/lib/index.css'
import docxPath from "../../../public/pdfs/abc.docx";
export default {
  components:{
    VueOfficeDocx
  },
  data(){
    return {
      docx: docxPath
    }
  },
  methods:{
    rendered(){
      console.log("渲染完成")
    }
  }
}
</script>

<style scoped>

.docx-container {
  padding: 0px;
}
</style>