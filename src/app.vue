<template lang="pug">
  #app
    h1
      | Text
      small {{msg}}
    form
      .controlgroup
        input(name="title", placeholder="Title", v-model="post.title")
      .controlgroup
        textarea(name="content", placeholder="Content", v-model="post.content")
      button(@click.prevent.stop="submit") Submit
    h2 Post List
    ul.list
      li(v-for="post in posts") {{post.title}} : {{post.content}}

</template>

<script>
import request from 'superagent'
export default {
  name: 'Application',
  data () {
    return {
      msg: 'Hello Sails.js + Vue.js',
      post: {
        title: '',
        content: ''
      },
      posts: null
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    fetchData () {
      request
      .get('/post')
      .end((err, res) => {
        if (err || !res.ok) {
          console.log(err)
        } else {
          this.posts = res.body
        }
      });
    },
    submit () {
      request
      .post('/post')
      .send({ title: this.post.title, content: this.post.content })
      .end((err, res) => {
        if (err || !res.ok) {
          console.log(err)
        } else {
          console.log(JSON.stringify(res.body))
        }
      });
    }
  }
}
</script>

<style lang="sass">
@import './assets/styles/normalize';
h1 {
  font-family: Helvetica, sans-serif;
  small {
    display: block;
  }
}
</style>
