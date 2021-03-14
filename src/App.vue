<template>
  <div id="app">
    <div id="main-column">
      <drawer :open.sync="openLeft">
        <h5>Left Drawer</h5>
      </drawer>
      <drawer :open.sync="openRight" right>
        <search />
      </drawer>
      <div id="header">
        <div class="title">Vue PDFTron Custom UI</div>
        {{zoom}}%
        <button id="zoom-out-button" title="zoom out" @click="zoomOut"><app-icon name="minusCircle"/></button>
        <button id="zoom-in-button" title="zoom in" @click="zoomIn"><app-icon name="plusCircle"/></button>
        <button id="fit-width-button" title="fit width" @click="fitWidth">fit width</button>
        <button id="fit-page-button" title="fit page" @click="fitPage">fit page</button>
        Current Page: {{currentPage}} of {{pageCount}} &nbsp;&nbsp;
        <button id="page-previous" title="previous page" @click="pagePrevious"><app-icon name="arrowUp"/></button>
        <button id="page-next" title="next page" @click="pageNext"><app-icon name="arrowDown"/></button>
        <button id="search" title="search" @click="openRight = !openRight"><app-icon name="search"/></button>
        {{nextMode}}
      </div>
      <div class="flexbox-container" id="scroll-view" :style="viewerStyle" ref=scrollView>
        <div id="viewer" ref=viewer />
      </div>
    </div>
  </div>
</template>

<script>
import AppIcon from '@/components/AppIcon'
import Drawer from '@/components/Drawer'
import Search from '@/components/Search'
export default {
  name: 'App',
  components: {AppIcon, Drawer, Search},
  computed: {
    viewerStyle() {
      const leftMargin = this.openLeft ? '75%' : undefined
      const rightMargin = this.openRight ? '-75%' : undefined
      return {...(leftMargin && {'margin-left': `${leftMargin}`}), ...(rightMargin && {'margin-right': `-${rightMargin}`})}
    },
    nextMode() {
      return '' //this.fitMode === this.docViewer.FitMode.FitWidth ? 'FitPage' : 'FitWidth'
    }
  },
  data() {
    return {
      docViewer: undefined,
      annotManager: undefined,
      searchContainerOpen: false,
      currentPage: 1,
      pageCount: 0,
      openLeft: false,
      openRight: false,
      fitMode: undefined,
      zoom: 100
    }
  },
  methods: {
    fitWidth() {
      this.docViewer.setFitMode(this.docViewer.FitMode.FitWidth);
    },
    fitPage() {
      this.docViewer.setFitMode(this.docViewer.FitMode.FitPage);
    },
    zoomIn() {
      this.docViewer.zoomTo(this.docViewer.getZoom() + 0.25)
    },
    zoomOut() {
      this.docViewer.zoomTo(this.docViewer.getZoom() - 0.25)
    },
    pagePrevious() {
      if(this.currentPage === 1) return
      this.docViewer.setCurrentPage(this.currentPage -1)
    },
    pageNext() {
      if(this.currentPage === this.pageCount) return
      this.docViewer.setCurrentPage(this.currentPage + 1)
    }
  },
  mounted () {
    const CoreControls = window.CoreControls;
    CoreControls.setWorkerPath('/webviewer/core');
    CoreControls.enableFullPDF(true);

    this.docViewer = new CoreControls.DocumentViewer();
    this.docViewer.setScrollViewElement(this.$refs.scrollView);
    this.docViewer.setViewerElement(this.$refs.viewer);
    this.docViewer.setOptions({ enableAnnotations: true });
    this.docViewer.setFitMode(this.docViewer.FitMode.FitPage);

    this.docViewer.loadDocument('files/compressed.tracemonkey-pldi-09.pdf');

    this.docViewer.on('documentLoaded', () => {
        console.log('document loaded');
        this.docViewer.zoomTo(1)
        // this.zoom = Math.round( zoomLevel * 100)this.docViewer.getZoom()
        console.log(this.docViewer.getZoomLevel)
        this.pageCount = this.docViewer.getPageCount()
        this.docViewer.setToolMode(this.docViewer.getTool('AnnotationEdit'));
        this.annotManager = this.docViewer.getAnnotationManager();
      })
    this.docViewer.on('pageNumberUpdated', (pageNo) => {
      this.currentPage = pageNo
    })
    this.docViewer.on('fitModeUpdated', fitMode => {
      this.fitMode = fitMode
    })
    this.docViewer.on('zoomUpdated', zoomLevel => {
      this.zoom = Math.round( zoomLevel * 100)
      console.log('zoom updated', this.zoom)
    })
  },
  beforeDestroy () {
    if(this.docViewer) {
      this.annotManager = undefined
      this.docViewer.dispose()
    }
  }
}
</script>
