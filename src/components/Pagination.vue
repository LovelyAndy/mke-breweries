<template>
  <div class="_pagination-wrapper">
    <div v-if="showPreviousLink" class="_pagination-btn" @click="updatePage(currentPage - 1)">
      <svg
        width="25"
        height="25"
        viewBox="0 0 25 25"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M19.9814 12.6434H5.98145"
          stroke="black"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M12.9814 19.6434L5.98145 12.6434L12.9814 5.64337"
          stroke="black"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
    </div>
    <span style="font-size: 32px; padding: 0 8px">{{ currentPage }}</span>
    <div v-if="showNextLink" class="_pagination-btn" @click="updatePage(currentPage + 1)">
      <svg
        width="25"
        height="25"
        viewBox="0 0 25 25"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M5.60596 12.5667H19.606"
          stroke="black"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M12.606 5.56671L19.606 12.5667L12.606 19.5667"
          stroke="black"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Pagination',
  props: ['visibleBreweries', 'currentPage', 'perPage'],
  methods: {
    updatePage(pageNum) {
      this.$emit('page-update', pageNum)
    },
  },
  computed: {
    showPreviousLink() {
      return this.currentPage === 1 ? false : true
    },
    showNextLink() {
      return true
    },
  },
}
</script>

<style lang="sass" scoped>
._pagination-btn
  display: inline-flex
  justify-content: center
  align-items: center
  cursor: pointer
</style>