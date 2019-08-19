<template>
  <Layout>
    <br />
    <g-link to="/" class="link">&larr; Go Back</g-link>
    <div class="post-title">
      <h1>{{$page.post.title}}</h1>
      <p class="post-date">{{ $page.post.date}} | {{$page.post.timeToRead}} min read</p>
    </div>
    <div class="post-content">
      <p v-html="$page.post.content" />
    </div>
    <g-link to="/" class="link">&larr; Go Back</g-link>
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
  }
}
</page-query>

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