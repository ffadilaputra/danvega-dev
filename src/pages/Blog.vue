<template>
  <Layout>
    <div class="content">
      <h1 class="title is-2">Recent Articles</h1>
      <p>
        I like to write about anything thta I am currently working or something new that interests me. If you would like me to write about something or be a guest blogger on your blog please reach out to me on
        <a
          href="https://twitter.com/therealdanvega"
        >Twitter</a>.
      </p>
      <blog-post-search/>
      <div class="articles">
        <div v-for="post in $page.posts.edges" :key="post.node.id" class="article content">
          <h2 class="title is-4">
            <g-link :to="post.node.path">{{post.node.title}}</g-link>
          </h2>
          <small>{{ new Date(post.node.date).toLocaleDateString() }} • ☕️ {{post.node.timeToRead}} min read</small>
          <p v-if="post.node.excerpt">{{post.node.excerpt}}</p>
        </div>
      </div>
    </div>
    <BulmaPagination
      baseUrl="/blog"
      :currentPage="$page.posts.pageInfo.currentPage"
      :totalPages="$page.posts.pageInfo.totalPages"
      :maxVisibleButtons="5"
      v-if="$page.posts.pageInfo.totalPages > 1"
    />
  </Layout>
</template>

<page-query>
query Blog ($page: Int) {
  posts: allPost(perPage: 5, page: $page) @paginate {
    pageInfo {
      totalPages
      currentPage
    }
    totalCount
    edges {
      node {
        id
        title
        slug
        excerpt
        cover
        timeToRead
        path
        date
        tags {
          id
          title
          path
        }
      }
    }
  }
}
</page-query>

<script>
import BulmaPagination from "@/components/BulmaPagination";
import BlogPostSearch from "@/components/BlogPostSearch";

export default {
  components: {
    BulmaPagination,
    BlogPostSearch
  }
};
</script>

<style>
.articles {
  margin: 20px 0;
}
.article h2 {
  margin-bottom: 0px !important;
}
.article h2 a:link,
.article h2 a:visited {
  color: #ff4e46;
}
</style>
