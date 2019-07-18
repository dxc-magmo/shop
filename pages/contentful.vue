<template>
  <div class="container">
    <div>
      <h1>Contentful</h1>
      <div>
        <NLink to="/">Home</NLink>
        <NLink to="/crystallize">Crystallize</NLink>
      </div>
      <div>
        <h2>Get all pages</h2>
        <ul>
          <li v-for="(page, index) in pages" :key="index">
            {{ page.fields.title }}
          </li>
        </ul>
        <h2>Get all blog posts</h2>
        <ul>
          <li v-for="(post, index) in posts" :key="index">
            {{ post.fields.title }}
          </li>
        </ul>
        <h2>Get all persons</h2>
        <ul>
          <li v-for="(person, index) in persons" :key="index">
            {{ person.fields.name }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import contentful from '~/plugins/contentful.js'

export default {
  head: {
    title: 'Contentful page'
  },
  asyncData({ params }) {
    // return contentful.getEntries({ content_type: 'page' }).then((result) => {
    //   return {
    //     pages: result.items
    //   }
    // })
    return Promise.all([
      contentful.getEntries({
        content_type: 'page'
      }),
      contentful.getEntries({
        content_type: 'blogPost'
      }),
      contentful.getEntries({
        content_type: 'person'
      })
    ])
      .then(([pages, posts, persons]) => {
        return {
          pages: pages.items,
          posts: posts.items,
          persons: persons.items
        }
      })
      .catch()
  }
}
</script>
