<template>
  <div>
    <header class="blog header">
      <div class="foreground">
        <div class="page-bar wrapper">
          <a href="/" class="person-name">おおたまを学ぶ</a>
          <Navigation></Navigation>
        </div>
        <div class="page-info wrapper">
          <h2>おおたまの仕事</h2>
        </div>
      </div>
    </header>

    <section class="body-container">
      <div class="items-bar wrapper">
      </div>
      <ul class="items-list wrapper">
        <li class="item" v-for="post in posts">
          <article-preview :post="post"></article-preview>
        </li>
      </ul>

    </section>

  </div>
</template>

<script>
import {createClient} from '~/plugins/contentful.js'
import Navigation from '~/components/navigation.vue'
import ArticlePreview from '~/components/article-preview.vue'

const client = createClient()

export default {
  asyncData ({env}) {
    return Promise.all([
      client.getEntries({
        'sys.id': env.CTF_PERSON_ID
      }),
      client.getEntries({
        'content_type': env.CTF_BLOG_POST_TYPE_ID,
        'fields.tags[in]': 'business',
        order: '-sys.createdAt'
      })
    ]).then(([entries, posts]) => {
      return {
        person: entries.items[0],
        posts: posts.items,
        tag: 'business'
      }
    }).catch(console.error)
  },
  components: {
    ArticlePreview,
    Navigation
  }
}
</script>
