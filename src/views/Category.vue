<template>
  <div>
    <div v-if=" items && items.length > 0" class="main-container-cat">
      <v-select
        :items="sortingOption"
        label="Sort by"
        dense
        style="width:200px;margin:20px auto; font-size: 14px"
        v-model="sort"
        @change="sorted"
      ></v-select>
      <div class="cat-gallery">
        <div v-for="(item, index) in items" :key="index">
          <Item :item="item" :isFav="false" />
        </div>
      </div>
    </div>
    <div v-else class="no-cat">
      <div>No Books found for that category.</div>
    </div>

    <Footer />
  </div>
</template>

<script>
import apiService from '@/apiConfig/itemService'
import Footer from '../components/commons/footer'
import Item from '../components/commons/item'
export default {
  data() {
    return {
      category: '',
      sort: '',
      items: [],
      sortingOption: ['High Rating to Low', 'Low Rating to High', 'High Price to Low', 'Low Price to High', 'Newest to Oldest', 'Oldest to Newest']
    }
  },
  components: {
    Footer, Item
  },
  methods: {
    getCategoryData() {
      this.category = this.$route.query.category
      apiService.fetchCategoryData({ category: this.category })
        .then(response => {
          this.items = response.data
        })
        .catch(error => {
          if (error && error.response !== 'undefined') {
            this.$store.dispatch('showSnackbar', { show: true, color: 'error', text: error.response.data.message || 'Something went wrong!' })
          }
        })
    },

    sorted() {
      apiService.sortData({ category: this.category, sortby: this.sort })
        .then(response => {
          this.items = response.data
        })
        .catch(error => {
          if (error && error.response !== 'undefined') {
            this.$store.dispatch('showSnackbar', { show: true, color: 'error', text: error.response.data.message || 'Something went wrong!' })
          }
        })
    }

  },
  beforeCreate() {
    this.$store.dispatch('changePage', '/category')
  },
  created() {

  },
  mounted() {
    this.getCategoryData()
  },
  watch: {
    '$route.fullPath': function () {
      this.getCategoryData()
      this.sorted()
    }
  }

}
</script>
<style scoped>
.remove-button {
  margin: 20px auto;
  color: rgba(236, 112, 99, 1);
  border: 1px solid rgba(236, 112, 99, 1);
  border-radius: 5 px;
  width: 75%;
}
.main-container-cat {
  padding-bottom: 15rem;
  display: flex;
  flex-direction: column;
}

.no-cat {
  height: 92vh;
  display: flex;
  justify-content: center;
  padding-top: 10vh;
}

.cat-gallery {
  width: 85%;
  margin: 25px auto;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
</style>
