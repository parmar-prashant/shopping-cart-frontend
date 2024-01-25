<template>
    <div class="border border-gray-700 rounded-md w-80 bg-white py-4">
        <div class="flex flex-row items-center justify-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
                class="w-6 h-6 mr-10 cursor-pointer" @click="previous">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5" />
            </svg>
            <div class="imageZoom" @mousemove="magnifyImage" @mouseleave="isZoomed = false" @click="openModal">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                    stroke="currentColor" class="imageIcon w-12 h-12" v-if="product.images.main[this.currentIndex].isVideo">
                    <path stroke-linecap="round" stroke-linejoin="round"
                        d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.347a1.125 1.125 0 0 1 0 1.972l-11.54 6.347a1.125 1.125 0 0 1-1.667-.986V5.653Z" />
                </svg>
                <img :src="product.images.main[this.currentIndex].path" class="w-60 h-60" ref="zoomImage" />
                <div v-show="!product.images.main[this.currentIndex].isVideo && isZoomed" class="zoomedImage"
                    ref="zoomedImage">
                    <img :src="product.images.zoomed[this.currentIndex].path" class="w-60 h-60" />
                </div>
            </div>
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
                class="w-6 h-6 ml-10 cursor-pointer" @click="next">
                <path stroke-linecap="round" stroke-linejoin="round" d="m8.25 4.5 7.5 7.5-7.5 7.5" />
            </svg>
        </div>
        <div class="text-center mt-10 hidden sm:block">
            <Thumbnail :thumbnails="product.images.thumbnail" :contentType="isSmallScreen ? 'image' : 'all'"
                @syncCurrentIndex="displaySelectedMedia" :currentIndex="currentIndex"></Thumbnail>
        </div>

        <modal v-show="showModal" @closeModal="showModal = false">
            <Tab :tabs="tabs">
                <template v-slot:image>
                    <div class="grid grid-rows-2 relative sm:grid-cols-4">
                        <div class="flex col-span-3 items-center justify-center m-2">
                            <img :src="product.images.zoomed[currentIndex].path" class="w-60 h-80 sm:w-64 h-80" />
                        </div>
                        <div class="p-5 border-4">
                            <p class="font-bold text-md">{{ productNameWithColorFormat }}</p>
                            <Thumbnail :thumbnails="product.images.thumbnail" contentType="image"
                                @syncCurrentIndex="displaySelectedMedia" :currentIndex="currentIndex"></Thumbnail>
                        </div>
                    </div>
                </template>
                <template v-slot:video>
                    <div class="grid grid-rows-2 relative sm:grid-cols-4 hidden sm:block">
                        <div class="flex col-span-3 items-center justify-center m-2">
                            <video class="videoPlayer" controls controlsList="nodownload nofullscreen noremoteplayback"
                                :key="product.images.main[currentIndex].id">
                                <template v-if="product.images.main[currentIndex].id !== undefined">
                                    <source :src="product.videos[product.images.main[currentIndex].id].url"
                                        type="video/mp4">
                                </template>
                            </video>
                        </div>
                        <div class="p-5 border-4">
                            <p class="font-bold text-md">{{ productNameWithColorFormat }}</p>
                            <Thumbnail :thumbnails="product.images.thumbnail" contentType="video"
                                @syncCurrentIndex="displaySelectedMedia" :currentIndex="currentIndex"></Thumbnail>
                        </div>
                    </div>
                </template>
            </Tab>
        </modal>
    </div>
</template>

<script>
import Modal from './custom/Modal.vue';
import Tab from './custom/Tab.vue';
import Thumbnail from './shared/Thumbnail.vue';

export default {
    name: 'ProductImage',
    components: {
        Modal,
        Tab,
        Thumbnail
    },
    data() {
        return {
            currentIndex: 0,
            selectedMedia: '',
            showModal: false,
            isZoomed: false,
            isSmallScreen: false,
            tabs: [
                { id: "image", label: 'Image' , visible: true},
                { id: "video", label: 'Video' , visible: true},
            ],
        }
    },
    computed: {
        getImageArrayMaxLength() {
            var counter = 0;
            this.product.images.main.map((item) => {
                if(!item.isVideo) counter++;
            });
            return counter;
        },
        getVideoArrayMaxLength() {
            var counter = 0;
            this.product.images.main.map((item) => {
                if(item.isVideo) counter++;
            });
            return counter;
        },
        productNameWithColorFormat() {
            return this.product.details.name + ' (Color: ' + this.product.details.color + ')';
        }
    },
    methods: {
        previous() {
            if (this.currentIndex > 0) this.currentIndex--;
        },
        next() {
            var maxLength = 0;
            this.isSmallScreen ? maxLength = this.getImageArrayMaxLength : maxLength = this.getVideoArrayMaxLength;
            if (this.currentIndex < (maxLength - 1)) this.currentIndex++;
        },
        openModal() {
            this.showTabBasedOnScreenSize();
            this.showModal = true;
        },
        showTabBasedOnScreenSize() {
            this.isSmallScreen ? this.tabs[1].visible = false : this.tabs[1].visible = true;
        },
        magnifyImage(event) {
            const zoomedImage = this.$refs.zoomedImage;
            const zoomImage = this.$refs.zoomImage;

            const rect = zoomImage.getBoundingClientRect();
            const x = event.clientX - rect.left;
            const y = event.clientY - rect.top;

            zoomedImage.style.transformOrigin = `${x}px ${y}px`;
            zoomedImage.style.transform = `scale(3, 3)`;
            zoomedImage.style.cursor = 'pointer';
            this.isZoomed = true;
        },
        displaySelectedMedia(index) {
            this.currentIndex = index;
        },
        screenSizeTracker() {
            this.currentIndex = 0;
            this.isSmallScreen = window.innerWidth < 640;
        }
    },
    mounted() {
        this.screenSizeTracker();
        window.addEventListener('resize', this.screenSizeTracker);
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.screenSizeTracker);
    },
    props: {
        product: {
            type: Object,
            requred: true
        }
    }
}
</script>

<style>
.imageZoom {
    position: relative;
    overflow: hidden;
}

.zoomedImage {
    position: absolute;
    top: 0;
    left: 0;
    pointer-events: none;
    transform: scale(1);
    transition: transform 0.3s ease;
}

.imageIcon {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: #fff;
    background-color: rgba(0, 0, 0, 0.2);
    display: block;
    max-width: 100%;
}

.videoPlayer {
    width: 300px;
    height: 300px;
}</style>