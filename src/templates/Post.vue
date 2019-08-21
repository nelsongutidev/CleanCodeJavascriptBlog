<template>
  <Layout>
    <br />

    <div v-if="language === 'en'">
      <g-link to="/" class="link">&larr; Go Back</g-link>
    </div>
    <div v-else>
      <g-link to="/es" class="link">&larr; Atras</g-link>
    </div>

    <div class="post-title">
      <h1>{{$page.post.title}}</h1>
      <p class="post-date">{{ $page.post.date}} | {{$page.post.timeToRead}} min read</p>
    </div>
    <div class="post-content">
      <p v-html="$page.post.content" />
    </div>
    <div v-if="language === 'en'">
      <g-link to="/" class="link">&larr; Go Back</g-link>
    </div>
    <div v-else>
      <g-link to="/es" class="link">&larr; Atras</g-link>
    </div>
  </Layout>
</template>

<page-query>
query Post ($path: String!) {
   post: post (path: $path) {
    id
    title
    content
    date (format: "D MMMM YYYY")
    timeToRead
    lang
  }
}
</page-query>

<script>
import PostList from "@/components/PostList";
import GithubCorner from "@/components/GithubCorner";
import LanguageToggle from "@/components/LanguageToggle";
import Header from "@/components/Header";

export default {
  computed: {
    language: function() {
      return this.$page.post.lang;
    }
  }
};
</script>

<style>
.post-title {
  text-align: center;
  font-size: 1.25rem;
  padding: 2em 0;
}

.post-date {
  font-size: 16px;
  font-weight: 400;
}

.post-content {
  font-size: 18px;
}

@media screen and (max-width: 650px) {
  .post-content {
    font-size: 0.8rem;
  }
}
</style>