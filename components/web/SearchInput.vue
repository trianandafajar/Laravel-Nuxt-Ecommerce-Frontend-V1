<template>
  <div class="search-wrap">
    <div class="input-group w-100">
      <input
        :type="mobile ? 'search' : 'text'"
        class="form-control search-form"
        :placeholder="placeholder"
        :value="value"
        @input="$emit('input', $event.target.value)"
        @keypress.enter="emitSearch"
      />
      <div class="input-group-append">
        <button @click="emitSearch" :class="['btn', mobile ? 'btn-warning' : 'btn-primary', 'search-button']">
          <i class="fa fa-search"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: String,
    mobile: Boolean,
    placeholder: {
      type: String,
      default: 'mau belanja apa hari ini ?'
    }
  },
  methods: {
    emitSearch() {
      this.$emit('search')
    }
  }
}
</script>

<style scoped>
.search-form {
  width: 100%;
}
</style> 