<template>
  <div class="menu_btn_con">
    <input
      @change="startRendering"
      ref="filechooser_ref"
      class="choose_file"
      type="file"
      accept=".pdf"
      hidden
    />
    <button @click="browseFile" class="menu_btn" type="button">Read</button>
  </div>
</template>

<script>

export default {
  components: {},
  data() {
    return {
    }
  },
  methods: {
    browseFile() {
      this.$refs.filechooser_ref.click();
    },

    startRendering() {
      this.$emit('showReader')
    },
  },
};
</script>

<style>

</style>