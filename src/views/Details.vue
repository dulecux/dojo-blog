<template>
  <div v-if="error">{{ error }}</div>
  <div v-if="post" class="post">
    <h3>{{ post.title }}</h3>
    <p class="pre">{{ post.body }}</p>
    <button @click="handleClick" class="delete">delete post</button>
  </div>
  <div v-else>
      <Spinner />
  </div>
</template>

<script>
import getPost from '@/composables/getPost'
import { useRoute, useRouter } from 'vue-router'
import { projectFirestore } from '../firebase/config'

import Spinner from '../components/Spinner.vue'


export default {
  components: { Spinner },
  props: ['id'],
  setup(props) {

    const route = useRoute()
    // console.log(route)
    // console.log(route.params)
    const router = useRouter()

    //props.id ili route.params.id
    const { error, post, load } = getPost(route.params.id)
    load()

    const handleClick = async () => {
      await projectFirestore.collection('posts')
        .doc(props.id)
        .delete()

      router.push({ name: 'Home'})
    }

    return { error, post, handleClick }
  },
}
</script>

<style scoped>
  .tags a {
    margin-right: 10px;
  }
  .post {
    max-width: 1200px;
    margin: 0 auto;
  }
  .post p {
    color: #444;
    line-height: 1.5em;
    margin-top: 40px;
  }
  .pre {
    white-space: pre-wrap;
  }

  button.delete {
    margin: 10px auto;
  }
</style>