<template>
    <button @click="redirectToUrl">跳转到另一个网址</button>
  </template>
  
  <script>
  export default {
    methods: {
      redirectToUrl() {
        window.location.href = 'http://dasp.swhysc.com:8090/scyd/#/news-app-sw'; // 将链接替换为你想要跳转的网址
      }
    }
  };
  </script>
  