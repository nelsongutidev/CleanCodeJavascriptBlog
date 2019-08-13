<template>
  <Layout>
    <header class="header">
      <h1 v-html="$page.metaData.siteName" />
      <p>
        A guide to producing readable, reusable, and refactorable
        software in JavaScript by
        <a
          class="link"
          href="https://twitter.com/ryconoclast"
        >Ryan McDermott</a>
      </p>
      <p>
        I am by all means
        <span>not</span> the author of any of these posts. Instead,
        I am a JS developer who has found HUGE value in them.
        I have created this site to make the information more accesible to more people and have it in blog form.
      </p>
    </header>
    <section class="posts">
      <PostList v-for="edge in $page.allPost.edges" :key="edge.node.id" :post="edge.node" />
    </section>
  </Layout>
</template>

<script>
import PostList from "@/components/PostList";
export default {
  components: {
    PostList
  },
  metaInfo: {
    title: "Clean Code Javascript"
  }
};
</script>

<page-query>
query {
  metaData {
    siteName
    siteDescription
  }
  allPost (sortBy: "order", order: ASC){
    totalCount
    edges {
      node {
        id
        title
        timeToRead
        description
        date (format: "D MMMM YYYY")
        path
        order
      }
    }

  }
}
</page-query>

<style>
.header {
  text-align: center;
}

.header p {
  font-weight: 200;
  text-align: left;
}

.header h1 {
  font-size: 3rem;
}

.header span {
  text-decoration: underline;
  text-decoration-color: hsl(0, 100%, 35%);
}
</style>
