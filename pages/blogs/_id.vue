<template>
  <div class="user">
    <div class="title">{{dataSource.title}}</div>
    <textarea id="MyID"></textarea>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    validate({ params }) {
      return !isNaN(+params.id)
    },
    async asyncData({ params, error }) {
      try {
        const { data } = await axios.get(`http://127.0.0.1:7001/api/blogs/${+params.id}`)
        console.log('data', data)
        return {
          dataSource: data.data,
        }
      } catch (e) {
        error({ message: 'User not found', statusCode: 404 })
      }
    },
    mounted() {
      var simplemde = new SimpleMDE({
        element: document.getElementById("MyID"),
        // status: false,
        // autoDownloadFontAwesome: false,
        // tabSize: 4,
//        renderingConfig: {
//          codeSyntaxHighlighting: true
//        },
      });
      // simplemde.toggleFullScreen();
      simplemde.value(this.dataSource.content);
      simplemde.togglePreview();
    },
  }
</script>

<style scoped>
  .user {
    font-family: sans-serif;
  }
  .user .title {
    font-size: 18px;
    line-height: 30px;
  }
</style>
