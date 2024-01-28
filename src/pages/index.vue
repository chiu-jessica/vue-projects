<template>
    <div id = "index-page">
      <img alt="Vue logo" id="image" src="https://m.media-amazon.com/images/M/MV5BZTkwZjU3MTctMGExMi00YjU5LTgwMDMtOWNkZDRlZjQ4NmZhXkEyXkFqcGdeQXVyNjAwNDUxODI@._V1_.jpg">
      <HelloWorld msg="Guardians of the Galaxy"/>
      <FaqItem v-for="(faq, index) in faqs" :key="index" :faq-object="faq"/>
      <ImgCarousel :img-array="images"/>
    </div>
  </template>
  
  <script>
  import HelloWorld from '../components/HelloWorld.vue'
  import FaqItem from '../components/FaqItem.vue'
  import ImgCarousel from '../components/ImgCarousel.vue'
  
  export default {
    name: 'IndexPage',
    components: {
      HelloWorld,
      FaqItem,
      ImgCarousel
  },
  data() {
    return {
      faqs: [
        {
          question:'What\'s Starlord\'s real name?', 
          answer: 'Peter Quill'
        },
        {
          question:'What\'s the raccoon\'s name?', 
          answer: 'Rocket'
        },
        {
          question:'Who\'s the villian in Guardians of the Galaxy', 
          answer: 'Ronan the Accuser'
        }
      ],
      images: [
        "https://lumiere-a.akamaihd.net/v1/images/pp_guardiansofthegalaxy_herobanner_mobile_21001_560ae126.jpeg?region=0,0,640,480",
        "https://www.indiewire.com/wp-content/uploads/2018/07/shutterstock_5886233bm.jpg?w=780",
        "https://pyxis.nymag.com/v1/imgs/401/4a9/f170e882648386057150dde12cc1f5b6e4-21-guardians-of-the-galaxy-3.rsquare.w700.jpg"
      ]
    }
  }
  }
  </script>
  
  <style>
  #index-page {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  
  .material-symbols-outlined {
    font-variation-settings:
    'FILL' 0,
    'wght' 400,
    'GRAD' 0,
    'opsz' 48
  }
  
  #image {
    height:370px;
    width:300px;
    object-fit:cover;
    object-position:50% 80%;
  }
  </style>
  