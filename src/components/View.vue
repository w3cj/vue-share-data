<template>
  <div>
    <button v-if="loaded && !uploading && !uploaded" @click="renderImage()" type="button" class="btn btn-warning" :disabled="uploading">
      Upload to Imgur
    </button>
    <div v-if="uploading">
      <h5>Uploading...</h5>
      <img class="img-responsive" src="https://media2.giphy.com/media/6nGE3BmUlhs3e/giphy.gif" alt="">
    </div>
    <h5><a :href="imageURL">{{imageURL}}</a></h5>
    <div id="image-container">
      <img id="rendered-image" src="" alt="">
    </div>
    <div v-if="!rendered" id="quote-container" class="quote-container">
      <img id="ron-img" src="https://images.weserv.nl/?url=az616578.vo.msecnd.net/files/2016/06/09/6360103332924270401274957217_Ron.jpg" alt="">
      <p class="quote-text">{{quote}}</p>
    </div>
    <br>
    <br>
    <br>
  </div>
</template>

<script>
import { getQuote } from './QuoteManager';

export default {
  name: 'view',
  data() {
    return {
      loaded: false,
      quote: 'No quote loaded...',
      rendered: false,
      imageURL: '',
      uploading: false,
      uploaded: false,
    };
  },
  methods: {
    renderImage() {
      const self = this;
      /* eslint-disable */
      html2canvas(document.querySelector('#quote-container'), {
        useCORS: true,
        onrendered(canvas) {
          var dataURL = canvas.toDataURL();
          self.uploadImage(dataURL);
          document.getElementById('rendered-image').src = dataURL;
          self.rendered = true;
        }
      });
    },
    uploadImage(image) {
      const body = {
        image: image.split(',')[1],
        type: 'base64',
        title: this.quote,
      };
      this.uploading = true;
      fetch('https://api.imgur.com/3/image', {
        method: 'POST',
        body: JSON.stringify(body),
        headers: {
          Authorization: 'Client-ID 63c969f7bcad2cf',
          'Content-Type': 'application/json',
        },
      }).then(res => res.json()).then((result) => {
        this.imageURL = result.data.link;
        this.uploading = false;
        this.uploaded = true;
      });
    },
  },
  mounted() {
    const quote = getQuote();
    if (quote) {
      this.quote = quote;
      this.loaded = true;
    }
  },
};
</script>

<style media="screen">
  .quote-container {
    position: relative;
    overflow: hidden;
    height: 100vh;
  }

  .quote-container img {
    position: absolute;
    top: 0;
    left: 0;
    width: 80vw;
    height: auto;
  }

  .quote-text {
    font-family: 'Impact';
    text-shadow: 1px 1px 2px black, 0 0 1em black, 0 0 0.2em black;
    font-size: 3em;
    color: white;
    position: absolute;
    margin-left: 0.5em;
    height: 100%;
    width: 40%;
  }

  #rendered-image {
    position: relative;
  }
</style>
