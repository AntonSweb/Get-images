<template>
    <div class="swiper-container swiper-ing">
        <div class="swiper-wrapper">
            <div class="swiper-slide" v-for="src in arrImgSrc">
                <figure class="ing__slide">
                    <img :src="src" alt="ing_carousel_img" class="ing__slide-photo" />
                    <figcaption class="ing__slide-title"></figcaption>
                </figure>
                <p class="ing__slide-desc"></p>
                <a href="#" class="ing__slide-link"></a>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        name: "get-img.vue",
        data (){
            return {
                swiperIng: {},
                arrImgSrc: [],
                html: '',
            }
        },
        mounted (){
            this.getAllImages();
        },
        methods: {
            initialSwiper: function (){
                this.swiperIng = new Swiper('.swiper-ing', {
                    navigation: {
                        nextEl: '.swiper-button-next',
                        prevEl: '.swiper-button-prev',
                    }
                });
            },
            getAllImages (){
                let that = this;
                axios.post('/img/ingredients')
                    .then(response => {
                        that.arrImgSrc = that.parseHtml(response.data)
                    })
                    .then(() => {
                        that.initialSwiper()
                    })
                    .catch(error => {
                        console.log(error)
                    });
            },
            parseHtml (html){
                let arrImg = [];
                this.html = document.createElement('html');
                this.html.innerHTML = html;
                this.html = this.html.querySelectorAll('[href *= ".jpg"]');

                for (let key in this.html) {
                    if (this.html[key].text !== undefined) {
                        arrImg[key] = '/img/ingredients/' + this.html[key].text;
                    }
                }
                return arrImg;
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>