<template>
  <div class="user">
    <p>{{dataSource.id}}</p>
    <p>{{dataSource.title}}</p>
    <p>{{dataSource.content}}</p>
    <p>{{dataSource.created_time}}</p>
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
    }
  }
</script>

<style scoped>
  .user {
    text-align: center;
    margin-top: 100px;
    font-family: sans-serif;
  }
</style>
