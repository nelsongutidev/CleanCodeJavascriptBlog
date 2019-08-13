<template>
  <div class="post-list">
    <hr class="line" />
    <p class="date" v-html="post.date" />
    <h1 class="title" v-html="post.title" />
    <p class="description" v-html="post.description" />
    <b>{{post.timeToRead}} min read</b> &nbsp;
    <g-link :to="post.path" class="read">Read More...</g-link>
  </div>
</template>

<script>
export default {
  props: ["post"]
};
</script>

<style>
.line {
  border: 0.5px solid rgb(28, 70, 107);
  margin: 30px 0;
}

.date {
  font-weight: 300;
}

.read {
  padding: 7px;
  color: #333;
  text-decoration: none;
}

.read:hover {
  border-bottom: 3px solid hsl(208, 59%, 80%);
  background-color: hsl(208, 59%, 20%);
  color: whitesmoke;
}
</style>
