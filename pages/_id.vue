<template>
  <div>
    <p>{{ shop.name }}</p>
    <p>
      <a :href="shop.url" target="_blank" rel="noopener">ページ</a>
    </p>
    <p>{{ shop.budget }}</p>
  </div>
</template>

<script lang="ts">
import Vue from "vue"

export default Vue.extend({
  data() {
    return {
      shop: {},
      error: false,
    }
  },
  async mounted() {
    try {
      const { data } = await this.$axios.get(
        `${process.env.apiurl}/api/gourmet/v1/`,
        {
          params: {
            key: process.env.apikey,
            id: this.$route.params.id,
            format: "json",
          },
        }
      )
      const shopData = data.results.shop[0]
      this.shop = {
        name: shopData.name,
        url: shopData.urls.pc,
        budget: shopData.budget.average,
      }
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
