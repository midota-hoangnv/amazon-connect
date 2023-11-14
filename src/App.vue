<template>
  <div>
    <h1>PDF.js Previous/Next example</h1>
    <p>Please use <a href="https://mozilla.github.io/pdf.js/getting_started/#download"><i>official releases</i></a> in
      production environments.</p>

    <div>
      <button @click="downloadPDF">Download</button>
      <button @click="onPrevPage">Previous</button>
      <button @click="onNextPage">Next</button>
      &nbsp; &nbsp;
      <span>Page: <span id="page_num">{{ pageNum }}</span> / <span id="page_count">{{ pdfDoc ? pdfDoc.numPages : 0 }}</span></span>
    </div>

    <div>
      <div class="fakeRect" @click="handleFakeRectClick"></div>
      <canvas id="the-canvas"></canvas>
    </div>
    <div>
      <canvas class="d-none" id="the-canvas2"></canvas>
    </div>
    <canvas class="d-none measureTextWidth"></canvas>
  </div>
</template>

<script>
import { PDFDocument, rgb, StandardFonts } from "pdf-lib";
import { jsPDF } from "jspdf";

export default {
  data() {
    return {
      pdfDoc: null,
      pageNum: 1,
      pageRendering: false,
      pageNumPending: null,
      scale: 1,
      canvas: null,
      ctx: null,
      signName: '',
    };
  },
  mounted() {
    this.initializePdf();
  },
  methods: {
    initializePdf() {
      const url = '/example.pdf';
      this.canvas = document.getElementById('the-canvas');
      this.ctx = this.canvas.getContext('2d');
      console.log('test', window.globalThis, window.pdfjsLib);
      const { pdfjsLib } = window.globalThis;
      if (pdfjsLib && pdfjsLib.GlobalWorkerOptions) {
        pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://mozilla.github.io/pdf.js/build/pdf.worker.mjs';
        pdfjsLib.getDocument(url).promise.then(pdfDoc => {
          this.pdfDoc = pdfDoc;
          this.renderPage(this.pageNum);
        });
      }
    },
    renderPage(num) {
      this.pageRendering = true;
      this.pdfDoc.getPage(num).then(page => {
        const viewport = page.getViewport({ scale: this.scale });
        this.canvas.height = viewport.height;
        this.canvas.width = viewport.width;

        const renderContext = {
          canvasContext: this.ctx,
          viewport: viewport,
        };
        const renderTask = page.render(renderContext);

        renderTask.promise.then(() => {
          this.pageRendering = false;
          if (this.pageNumPending !== null) {
            this.renderPage(this.pageNumPending);
            this.pageNumPending = null;
          }
          this.ctx.beginPath();
          this.ctx.strokeStyle = "#080808";
          const x = 350, y = 500, width = 100, height = 100;
          this.ctx.strokeRect(x, y, width, height);

          this.ctx.beginPath();
          this.ctx.strokeStyle = "#FF0000";
          this.ctx.lineWidth = 2;
          this.ctx.arc(400, 550, 40, 0, 2 * Math.PI);
          this.ctx.stroke();
        });
      });

      document.getElementById('page_num').textContent = num;
    },
    queueRenderPage(num) {
      if (this.pageRendering) {
        this.pageNumPending = num;
      } else {
        this.renderPage(num);
      }
      setTimeout(() => {
        this.ctx.beginPath();
        this.ctx.fillStyle = '#FF0000';
        this.ctx.font = "20px Arial";
        this.ctx.fillText(this.signName, 372, 556);
      }, 100);
    },
    onPrevPage() {
      if (this.pageNum > 1) {
        this.pageNum--;
        this.queueRenderPage(this.pageNum);
      }
    },
    onNextPage() {
      if (this.pageNum < this.pdfDoc.numPages) {
        this.pageNum++;
        this.queueRenderPage(this.pageNum);
      }
    },
    handleFakeRectClick() {
      this.renderPage(1);
      setTimeout(() => {
        this.handleSign();
      }, 100);
    },
    handleSign() {
      let person = prompt("Please enter your sign name", "HUNG");
      if (person != null) {
        person = "HUNG";
        this.signName = person;
        const textCanvas = document.querySelector('.measureTextWidth');
        const context = textCanvas.getContext('2d');
        context.font = '20px Arial';
        // const metrics = context.measureText(this.signName);
        // const textWidth = metrics.width;
        // const textHeight = metrics.actualBoundingBoxAscent + metrics.actualBoundingBoxDescent;
        this.ctx.beginPath();
        this.ctx.fillStyle = '#FF0000';
        this.ctx.font = "20px Arial";
        this.ctx.fillText(this.signName, 372, 556);
      }
    },
    downloadPDF() {
      var imgData = this.canvas.toDataURL("image/jpeg", 1.0);
      var pdf = new jsPDF();
      pdf.addImage(imgData, 'JPEG', 0, 0);
      pdf.save("download.pdf");
    },
    async modifyPdf() {
      // Fetch an existing PDF document
      const url = '/example.pdf'
      const existingPdfBytes = await fetch(url).then(res => res.arrayBuffer())

      // Load a PDFDocument from the existing PDF bytes
      const pdfDoc = await PDFDocument.load(existingPdfBytes)

      // Embed the Helvetica font
      const helveticaFont = await pdfDoc.embedFont(StandardFonts.Helvetica)

      // Get the first page of the document
      const pages = pdfDoc.getPages()
      const firstPage = pages[0]
      const secondPage = pages[1]

      // Get the width and height of the first page
      const { width, height } = firstPage.getSize()
      const context = firstPage.getContentStream();
      this.drawSignature(firstPage, width, height, context, helveticaFont)
      this.drawSignature(secondPage, width, height, context, helveticaFont)

      // Serialize the PDFDocument to bytes (a Uint8Array)
      const pdfBytes = await pdfDoc.save()

      // Trigger the browser to download the PDF document
      download(pdfBytes, "example.pdf", "application/pdf");
    },
    async drawSignature(target, width, height, context, helveticaFont) {
      const rightCoordinate = 250;
      const rectX = width - rightCoordinate;
      const rectY = 200;

      const circleX = rectX + 35; // X-coordinate of the circle center
      const circleY = rectY + 35; // Y-coordinate of the circle center
      const rectWidth = 70;
      const rectHeight = 70;

      target.drawEllipse({
        context,
        x: circleX,
        y: circleY, // Invert Y-axis for PDF coordinates
        borderWidth: 2, // Border width (optional)
        borderColor: rgb(1, 0, 0), // Border color (optional)
        xScale: 30, // Scale,
        yScale: 30, // Scale,
      });



      target.drawRectangle({
        context,
        x: width - rightCoordinate,
        y: rectY,
        width: rectWidth,
        height: rectHeight,
        borderWidth: 1,
        borderColor: rgb(0, 0, 0),
      });

      target.drawText(this.signName, {
        x: rectX + 13,
        y: rectY + 30,
        size: 15,
        font: helveticaFont,
        color: rgb(1, 0, 0),
      })
    }
  },
};
</script>

<style>
#the-canvas {
  border: 1px solid black;
  direction: ltr;
}

.d-none {
  display: none;
}

.fakeRect {
  position: absolute;
  background: transparent;
  height: 100px;
  width: 100px;
  left: 359px;
  bottom: 150px;
  z-index: 1;
  content: "";
}

canvas {
  position: relative;
}
</style>
