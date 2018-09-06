<template>
  <div>
    <header class="home header">
      <div class="picture">
          <!--<img :src="person.fields.image.fields.file.url + '?w=1200'">-->
        </div>
        <div class="foreground">
          <div class="page-bar wrapper">
            <Navigation></Navigation>
          </div>
        </div>
    </header>

    <section class="body-container">
      <div class="items-bar wrapper">
        <h2>おおたまを学ぶコンテンツ</h2>
      </div>
      <ul class="items-list wrapper">
        <li class="item" v-for="item in items">
          <category-preview :item="item"></category-preview>
        </li>
      </ul>
<!--
      <ul class="items-list wrapper">
        <li class="item" v-for="post in posts">
          <article-preview :post="post"></article-preview>

        </li>
      </ul>
-->
      <!--<ul class="items-list wrapper">
      <li class="item" v-for="(item, index) in items" :key=index>
      <router-link v-bind:to=item.path>
        <div class="thumnail">
            <img :src=" item.imagepath + '?w=300'">
          </div>
         {{index +1}} ・ {{ item.title }}
      </router-link>
    </li>
  </ul>-->
    </section>
  </div>
</template>

<script>
import {createClient} from '~/plugins/contentful.js'
import Navigation from '~/components/navigation.vue'
import ArticlePreview from '~/components/article-preview.vue'
import CategoryPreview from '~/components/category-preview.vue'

const client = createClient()

export default {
  asyncData ({env}) {
    return Promise.all([
      client.getEntries({
        'sys.id': env.CTF_PERSON_ID
      }),
      client.getEntries({
        'content_type': env.CTF_BLOG_POST_TYPE_ID,
        'fields.tags[in]': 'parents',
        order: '-sys.createdAt'
      })
    ]).then(([entries, posts]) => {
      return {
        person: entries.items[0],
        posts: posts.items,
        tag: 'parents'
      }
    }).catch(console.error)
  },
  components: {
    Navigation,
    ArticlePreview,
    CategoryPreview
  },
  data: function () {
    return {
      items: [
        { title: 'しぜん', path: 'nature', imagepath: 'images/001.jpg' },
        { title: 'たてもの', path: 'building', imagepath: 'images/002.jpg' },
        { title: '歴史と文化', path: 'culture', imagepath: 'images/003.jpg' },
        { title: 'しごと', path: 'business', imagepath: 'images/004.jpg' },
        { title: 'ひと', path: 'human', imagepath: 'images/005.jpg' }
      ]
    }
  }
}
</script>

<style>

.home.header {
  overflow: hidden;
  position: relative;
  height: 10vw;
  min-height: 200px;
  max-height: 200px;
  background: #444;
  color: #fff;
}

.home .person-name:link,
.home .person-name:visited {
  color: #fff;
}

.home .menu a:link,
.home .menu a:visited {
  color: #fff;
}

.home .page-bar {
  border-bottom: 1px solid #4e4b5f;
}

.home .page-info {
  padding-top: 12%;
}

.home .page-info h2 {
  font-size: 1.5em;
}

.picture {
  position: absolute;
  z-index: 2;
  top: 0;
  bottom: 0;
  left: 0%;
  right: 0;
  width: 150%;
}

@media all and (min-width: 600px) {
  .picture {
    left: 20%;
    width: 100%;
  }
}

.foreground {
  overflow: hidden;
  position: relative;
  z-index: 3;
}

.foreground p {
  margin-bottom: 1em;
}

@media all and (min-width: 600px) {
  .foreground p {
    max-width: 250px;
  }
}

.social-icons {
  margin: 0;
  padding: 0;
}

.social-icons li {
  display: inline-block;
  margin-right: 1em;
}

.social-icons a {
  display: block;
}

.social-icons svg {
  display: block;
  max-width: 100%;
  fill: #fff;
  width: 1.5em;
  height: 1.5em;
}

.social-icons svg:hover {
  fill: #2199e8;
}

.thumbnail {
  margin-bottom: 1em;
}

</style>
