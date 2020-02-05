<template>
  <div class="form-check form-check-inline">
    <label class="form-check-label" for></label>
    <input type="checkbox" class="form-check-input" name="typePost" value="even" v-model="postType" /> Even
    <input
      type="checkbox"
      class="form-check-input"
      name="typePost"
      value="odd"
      v-model="postType"
    /> Odd
  </div>
</template>
<script>
export default {
  data() {
    return {
      postType: []
    };
  },
  watch: {
    postType() {
      this.$emit("postTypeChange", this.postType);
    }
  }
};
</script>
<style scoped>
</style>