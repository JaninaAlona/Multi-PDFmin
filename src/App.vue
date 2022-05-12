<template>
 <header>
    <div class="buttonbar">
      <Viewer />
      <div class="menu_btn_con">
        <button class="menu_btn" type="button">Create</button>
      </div>
      <div class="menu_btn_con">
        <button class="menu_btn" type="button">Split</button>
      </div>
    </div>
  </header>
</template>

<script>
import Viewer from './components/Viewer.vue'


export default {
  name: 'App',
  components: {
    Viewer,
  },
  data() {
    return {
      
    }
  },
  /* mounted() {
    let pdfJSLibrary = document.createElement('script')
    pdfJSLibrary.setAttribute('src', 'https://cdnjs.com/libraries/pdf.js')
    document.head.appendChild(pdfJSLibrary)
  } */
}
</script>

<style>

/*https://fonts.google.com/specimen/Radio+Canada#standard-styles*/
@import url('https://fonts.googleapis.com/css2?family=Radio+Canada:wdth,wght@81.5,300;81.5,392;81.5,573;100,300;100,400&display=swap');

body {
  margin: 0%;
}

#app {
  font-family: 'Radio Canada', sans-serif;
  font-style: "Regular";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

header {
  background: rgb(134, 192, 139);
  height: 3.8em;
}

.buttonbar {
  position: relative;
  top: 15px;
  left: 15px;
  display: flex;
  flex-wrap: wrap;
}

.menu_btn_con {
  margin-right: 15px;
}

.menu_btn {
  height: 2em;
  background: rgb(233, 169, 169);
  border: none;
  color:rgb(14, 94, 20);
  font-size: 12pt;
  width: 8em;
}
</style>
