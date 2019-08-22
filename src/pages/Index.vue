<template>
  <Layout>
    <header class="header">
      <g-link class="g-link" to="/es">Version en Español &rarr;</g-link>
      <h1 v-html="$page.metaData.siteName" />
      <Header :language="language" />
    </header>
    <section class="posts">
      <PostList v-for="edge in blogPosts" :key="edge.node.id" :post="edge.node" />
    </section>
  </Layout>
</template>

<script>
import PostList from "@/components/PostList";
import Header from "@/components/Header";

export default {
  data() {
    return {
      language: "en"
    };
  },
  computed: {
    blogPosts: function() {
      return this.$page.allPost.edges.filter(
        edge => edge.node.lang === this.language
      );
    }
  },
  components: {
    PostList,
    Header
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
        path
        order
        lang
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
  margin-top: 4rem;
  font-size: 2.5rem;
}

.header span {
  text-decoration: underline;
  text-decoration-color: hsl(0, 100%, 35%);
}

.g-link {
  position: absolute;
  top: 15px;
  left: 15px;
  color: rgb(28, 70, 107);
}
</style>
