<template>
  <v-row class="content-container">
    <template v-if="!error">
      <subpage-title-section-component :title="title + name" :fontSize="28" />

      <div class="shortcuts-list">
        <v-col v-for="item in data" :key="item.id" lg="12" style="padding: 0;">
          <div class="shortcut-item-section">
            <div class="shortcut-keys">
              <v-icon v-if="item.icon">mdi-windows</v-icon> {{ item.shortcut }}
            </div>
            <div class="shortcut-description">
              {{ item.description }}
            </div>
          </div>
        </v-col>
      </div>
    </template>
    <template v-else>
      <div style="margin-top: 80px;">
        fetch error
      </div>
    </template>
  </v-row>
</template>

<script>
import axios from 'axios'
import SubpageTitleSection from '../../components/subpage-title-section'

export default {
  components: {
    'subpage-title-section-component': SubpageTitleSection
  },
  data() {
    return {
      title: 'Skróty klawiaturowe - ',
      error: false
    }
  },
  asyncData({ params, error }) {
    return axios
      .get(`/shortcuts/${params.id}.json`)
      .then((res) => {
        return {
          id: params.id,
          name: res.data.name,
          data: res.data.items
        }
      })
      .catch(() => {
        return {
          error: true
        }
      })
  }
}
</script>

<style scoped>
.content-container {
  max-width: 800px;
  width: 100%;
  margin: auto;
  margin-bottom: 80px;
}
.shortcuts-list {
  padding: 0;
  margin-top: 60px;
  border-top: 1px solid #d9d9d9;
}
.shortcut-item-section {
  display: flex;
  letter-spacing: 1px;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom: 1px solid #d9d9d9;
}
.shortcut-item-section:hover {
  background-color: #f0f0f5;
}
.shortcut-keys {
  width: 40%;
  font-weight: 600;
}
.shortcut-description {
  width: 60%;
  padding-left: 10px;
}
</style>
