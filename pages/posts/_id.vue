<template>
  <v-row class="content-container">
    <template v-if="!error">
      <div class="post-content">
        <subpage-title-section-component
          :title="attributes.title"
          :marginTop="marginTop"
          :fontSize="28"
        />

        <div class="post" v-html="$md.render(model)"></div>
      </div>

      <posts-sidebar-menu-component />
    </template>
    <template v-else>
      <v-col lg="12">
        <div class="error-message">
          {{ errorMessage }}
        </div>
        <div>
          <nuxt-link to="/posts" class="link">
            Zobacz inne posty
          </nuxt-link>
        </div>
      </v-col>
    </template>
  </v-row>
</template>

<script>
import axios from 'axios'
import frontmatter from 'front-matter'
import SubpageTitleSection from '../../components/subpage-title-section'
import PostsSidebarMenu from '../../components/posts-sidebar-menu'

export default {
  components: {
    'subpage-title-section-component': SubpageTitleSection,
    'posts-sidebar-menu-component': PostsSidebarMenu
  },
  data() {
    return {
      title: '',
      error: false,
      ht: 'Jakub Gania Software - Post - ',
      errorMessage: 'Wystąpił błąd pobierania lub taki post nie istnieje',
      marginTop: 0
    }
  },
  async asyncData({ params }) {
    try {
      const { data } = await axios.get(
        `https://jakubgania.io/media/posts/${params.id}.md`
      )

      const frontmatterData = frontmatter(data)

      return {
        model: data,
        attributes: frontmatterData.attributes
      }
    } catch (error) {
      return {
        model: 'Taki post nie istnieje',
        error: true,
        attributes: {
          title: '',
          description: '',
          keywords: ''
        }
      }
    }
  },
  head() {
    return {
      title: this.ht + this.attributes.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.attributes.description
        },
        {
          name: 'keywords',
          content: this.attributes.keywords
        }
      ]
    }
  }
}
</script>

<style scoped>
.content-container {
  max-width: 1020px;
  width: 100%;
  margin: auto;
  margin-top: 120px;
}
.link {
  text-decoration: none;
  color: #b3b3b3;
}
.link:hover {
  color: #000;
}
.error-message {
  margin-top: 80px;
  color: red;
  font-weight: 800;
}
.post-content {
  max-width: 600px;
  padding: 0;
}
.post {
  font-family: 'Nunito', sans-serif;
  letter-spacing: 0.5px;
  line-height: 1.8;
  font-size: 16.8px;
  padding-top: 20px;
}
</style>
