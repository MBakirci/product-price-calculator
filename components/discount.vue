<template>
  <div class="form-group">
    <label>{{ label }}</label>
    <input
      v-model.number="value"
      type="text"
      class="form-control"
      placeholder="0"
      @change="update"
    />
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: ' '
    },
    value: {
      type: String,
      default: ' '
    }
  },
  methods: {
    update: function() {
      this.$emit('update', this.value)
    }
  }
}
</script>
