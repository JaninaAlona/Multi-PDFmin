<template>
  <div id="menubar">
    <div class="menu_btn_con">
      <button class="menu_btn" type="button">View</button>
    </div>
    <div class="menu_btn_con">
      <button class="menu_btn" type="button">Create</button>
    </div>
    <div class="menu_btn_con">
      <button class="menu_btn" type="button">Split</button>
    </div>
  </div>
</template>

<script>

export default {
  components: {
  },
  data() {
    return {
    
    }
  }
}
</script>

<style>
#menubar {
  background: rgb(134, 192, 139);
  display: flex;
  flex-direction: row;
  height: 80px;
}
.menu_btn_con {
  display: flex;
  flex-wrap: wrap;
  align-content: center;
  width: 100%;
  justify-content: space-evenly;
}
.menu_btn {
  width: 90%;
}
</style>