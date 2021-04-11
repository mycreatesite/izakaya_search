<template>
  <section>
    <h2>検索結果 （{{ shops.length }}件）</h2>
    <p v-if="error">データの取得に失敗しました</p>
    <ul>
      <li v-for="shop in shops" :key="shop.id">
        <NuxtLink :to="`/${shop.id}`">
          <p>{{ shop.id }}</p>
          <p>{{ shop.name }}</p>
        </NuxtLink>
      </li>
    </ul>
  </section>
</template>

<script lang="ts">
import Vue from "vue"

const getCurrentPos = () => {
  return new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(resolve, reject)
  })
}

export default Vue.extend({
  data() {
    return {
      shops: [],
      error: false,
    }
  },
  async mounted() {
    try {
      const pos: any = await getCurrentPos()
      const { data } = await this.$axios.get(
        `${process.env.apiurl}/gourmet/v1/`,
        {
          params: {
            key: process.env.apikey,
            lat: pos.coords.latitude,
            lng: pos.coords.longitude,
            keyword: "居酒屋",
            count: 100,
            format: "json",
          },
        }
      )
      this.shops = data.results.shop
    } catch (err) {
      this.setError()
    }
  },
  methods: {
    setError() {
      this.error = true
    },
  },
})
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
