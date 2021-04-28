<template>
  <div class="decimal" :class="{ 'decimal--error': validateInput }">
      <input type="number"
        v-model="val1"
        :min="1"
        :max="99"
        @change="calculateSum"
        class="decimal__input decimal__input--border"
      >
      <input type="number"
        v-model="val2"
        :min="1"
        :max="99"
        @change="calculateSum"
        class="decimal__input"
      >
      <button
        class="decimal__btn"
        @click.prevent="deleteItem"
        v-show="showDeleteButton"
      >
        <svg width="14" height="14" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M10.5 3.5l-7 7M3.5 3.5l7 7" stroke="#000" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
  </div>
</template>
<script>
export default {
  props: {
    index: {
      type: Number,
    },
    showDeleteButton: {
      type: Boolean,
    },
  },
  data() {
    return {
      val1: 0,
      val2: 0,
    }
  },
  methods: {
    calculateSum() {
      if (!this.val2 == 0) {
        const sum = this.val1 / this.val2 
        const index = this.index
        this.$emit('calculateSum', {index, sum})
      }
    },
    deleteItem() {
      this.$emit('delete', this.index)
    }
  },
  computed: {
    validateInput: function() {
      return (this.val1 && this.val2) ? false : true 
    },
  }
}
</script>
