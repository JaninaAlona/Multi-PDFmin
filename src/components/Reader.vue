<template>
  <div id="reader">
    <div id="reader_controls">
      <button id="go_previous" class="pages file_manip">
        Previous
      </button>
      <input
        v-model="pageInput"
        id="current_page"
        class="file_manip"
        type="number"
      />
      <button id="go_next" class="pages file_manip">
        Next
      </button>
      <button id="zoom_in" class="zoom file_manip">+</button>
      <input
        v-model="zoomInput"
        id="zoom_factor"
        class="file_manip"
        type="text"
      />
      <button id="zoom_out" class="zoom file_manip">-</button>
    </div>
    <div id="pdf_viewer_con">
      <div ref="pdfviewer_ref" id="pdf_viewer"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pdfState: {
        pdf: null,
        currentPage: 1,
        zoom: 1,
        pageHeight: [],
      },
      pageCounter: 1,
      pageInput: 1,
      zoomInput: "100%",
    }
  },
  methods: {
    resetRendering() {
      let pdfViewer = this.$refs.pdfviewer_ref;
      while (pdfViewer.firstChild) {
        pdfViewer.removeChild(pdfViewer.firstChild);
      }
      this.pdfState.pageHeight = [];
      this.pageCounter = 1;
    },

    resetSettingVals() {
      this.pageInput = 1;
      this.zoomInput = 100 + "%";
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

    resetToDefaults() {
      this.pageInput = 1;
      this.zoomInput = 100 + "%";
      //document.getElementById("pdf_viewer").style.display = "block";
      //document.getElementById("reader_controls").style.display = "flex";
    },

    renderPDF(e) {
      this.cleanUp();
      const file = e.target.files[0];
      const fileReader = new FileReader();
      const self = this;
      fileReader.onload = function () {
        const typedarray = new Uint8Array(this.result);
        self.resetSettingVals();
        const loadingTask = pdfjsLib.getDocument(typedarray);
        //missing rejection case in promise, pop up if PDF cannot be open
        loadingTask.promise.then(pdf => {
            self.resetToDefaults();
            self.pdfState.pdf = pdf;
            self.pdfState.pdf.getPage(1).then(self.renderAllPages);
        });
      };
      fileReader.readAsArrayBuffer(file);
    },

    renderAllPages(page) {
      let pdfViewer = this.$refs.pdfviewer_ref;
      let viewport = page.getViewport({
          scale: this.pdfState.zoom
      });

      let canvas = document.createElement("canvas");
      canvas.style.display = "block";
      canvas.style.marginBottom = "20px";

      const context = canvas.getContext('2d');
      canvas.width = viewport.width;
      canvas.height = viewport.height;

      this.pdfState.pageHeight.push(canvas.height);

      // Render PDF page into canvas context
      page.render({
          canvasContext: context,
          viewport: viewport
      });

      pdfViewer.appendChild(canvas);

      this.pageCounter++;
      if (this.pdfState.pdf != null && this.pageCounter <= this.pdfState.pdf._pdfInfo.numPages) {
          this.pdfState.pdf.getPage(this.pageCounter).then(this.renderAllPages);
      }
    },
  }
};
</script>

<style>
#pdf_viewer_con {
  display: flex;
  justify-content: center;
  width: 100%;
  height: 100%;
  background: #333;
  z-index: 8;
}

.file_manip {
    float: left;
    margin: 10px 5px 10px 10px;
    position: relative;
    z-index: 9;
}

.pages {
    width: 90px;
}

.zoom {
    width: 50px;
    font-weight: bold;
}

#reader_controls {
    background: rgb(161, 144, 172);
    display: flex;
    justify-content: center;
    width: 100%;
    z-index: 8;
}
</style>