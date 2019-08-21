<template>
  <Layout>
    <header class="header">
      <GithubCorner />
      <LanguageToggle @clicked="changeLanguage" />
      <!-- <button @click="dataFilter">data</button> -->
      <h1 v-html="$page.metaData.siteName" />

      <Header :language="language" />
    </header>
    <!-- <section class="posts">
      <PostList v-for="edge in $page.allPost.edges" :key="edge.node.id" :post="edge.node" />
    </section>-->
    <section class="posts">
      <PostList v-for="edge in blogPosts" :key="edge.node.id" :post="edge.node" />
    </section>
  </Layout>
</template>

<script>
import PostList from "@/components/PostList";
import GithubCorner from "@/components/GithubCorner";
import LanguageToggle from "@/components/LanguageToggle";
import Header from "@/components/Header";

export default {
  data() {
    return {
      language: "en",
      posts: []
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
    GithubCorner,
    LanguageToggle,
    Header
  },
  metaInfo: {
    title: "Clean Code Javascript"
  },
  methods: {
    changeLanguage: function() {
      if (this.language === "en") {
        this.language = "es";
      } else {
        this.language = "en";
      }
      console.log("hello");
    },
    dataFilter() {
      console.log(
        this.$page.allPost.edges.filter(
          edge => edge.node.lang === this.language
        )
      );
    }
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
</style>
