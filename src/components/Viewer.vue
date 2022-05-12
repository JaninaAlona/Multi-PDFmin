<template>
  <div class="menu_btn_con">
    <input
      @change="renderPDF"
      ref="filechooser_ref"
      class="choose_file"
      type="file"
      accept=".pdf"
      hidden
    />
    <button @click="browseFile" class="menu_btn" type="button">View</button>
  </div>
  <div v-if="showReader" id="reader">
    <div id="reader_controls">
      <button id="go_previous" class="pages btn btn-success file_manip">
        Previous
      </button>
      <input
        ref="current_page_ref"
        id="current_page"
        class="file_manip"
        value="1"
        type="number"
      />
      <button id="go_next" class="pages btn btn-success file_manip">
        Next
      </button>
      <button id="zoom_in" class="zoom btn btn-success file_manip">+</button>
      <input
        ref="zoom_factor_ref"
        id="zoom_factor"
        class="file_manip"
        value="100%"
        type="text"
      />
      <button id="zoom_out" class="zoom btn btn-success file_manip">-</button>
    </div>
    <div id="pdf_viewer_con">
      <div ref="pdf_viewer_ref" id="pdf_viewer"></div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      showReader: false,
      pdfState: {
        pdf: null,
        currentPage: 1,
        zoom: 1,
        pageHeight: [],
      },
      pageCounter: 1,
    };
  },
  methods: {
    browseFile() {
      this.$refs.filechooser_ref.click();
    },

    renderPDF(e) {
      this.cleanUp();
      const file = e.target.files[0];
      const fileReader = new FileReader();
      fileReader.onload = function () {
        const typedarray = new Uint8Array(this.result);
        const loadingTask = pdfjsLib.getDocument(typedarray);
        loadingTask.promise.then((pdf) => {
          this.resetToDefaults();
          this.pdfState.pdf = pdf;
          this.pdfState.pdf.getPage(1).then(this.renderAllPages);
        });
      };
      fileReader.readAsArrayBuffer(file);
      this.showReader = true;
    },

    cleanUp() {
      try {
        let pdf = this.pdfState.pdf;
      } catch (e) {
        if (e instanceof TypeError) {
        } else {
          this.pdfState.pdf = null;
        }
      } finally {
        this.pdfState.zoom = 1.0;
        this.resetRendering();
      }
    },

    resetRendering() {
      let pdfViewer = this.$refs.pdfviewer_ref;
      while (pdfViewer.firstChild) {
        pdfViewer.removeChild(pdfViewer.firstChild);
      }
      this.pdfState.pageHeight = [];
      this.pageCounter = 1;
    },

    resetToDefaults() {
      this.$refs.current_page_ref.value = 1;
      this.$refs.zoom_factor_ref.value = 100 + "%";
    },

    renderAllPages(page) {
      let pdfViewer = this.$refs.pdf_viewer_ref;
      let viewport = page.getViewport({
        scale: pdfState.zoom,
      });

      let canvas = document.createElement("canvas");
      canvas.style.display = "block";
      canvas.style.marginBottom = "20px";

      const context = canvas.getContext("2d");
      canvas.width = viewport.width;
      canvas.height = viewport.height;

      this.pdfState.pageHeight.push(canvas.height);

      // Render PDF page into canvas context
      page.render({
        canvasContext: context,
        viewport: viewport,
      });

      pdfViewer.appendChild(canvas);

      this.pageCounter++;
      if (
        this.pdfState.pdf != null &&
        this.pageCounter <= this.pdfState.pdf._pdfInfo.numPages
      ) {
        this.pdfState.pdf.getPage(this.pageCounter).then(this.renderAllPages);
      }
    },
  },
};
</script>

<style>
#pdf_viewer_con {
  display: flex;
  position: relative;
  top: 58px;
  justify-content: center;
  width: 100%;
  height: 100%;
  background: #333;
}

#pdf_viewer {
  display: block;
}

#reader_controls {
  display: flex;
}
</style>