<template>
    <div class="border border-gray-700 rounded-md w-80 bg-white py-4">
        <!--
            Image slider section with left and right button to navigate between the images and video.
        -->
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
                <img :src="product.images.main[this.currentIndex].path" class="w-60 h-60" ref="zoomImage"
                    @touchstart="onTouchStart" @touchmove="onTouchMove" @touchend="onTouchEnd" />
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

        <!--
            Thumbnail section which is a component used to display thumbnails of the given media files.
        -->
        <div class="text-center mt-10 hidden sm:block">
            <Thumbnail :thumbnails="product.images.thumbnail" :contentType="isSmallScreen ? 'image' : 'all'"
                @syncCurrentIndex="displaySelectedMedia" :currentIndex="currentIndex"></Thumbnail>
        </div>

        <!--
            Modal popup component to display media alongwidth name & color of the product using tabular format.
        -->
        <modal v-show="showModal" @closeModal="showModal = false">
            <Tab :tabs="tabs">
                <template v-slot:image>
                    <span class="text-xs italic text-rose-950">Double tap on image to zoom in/out.</span>
                    <div class="grid grid-rows-2 relative sm:grid-cols-4">
                        <div class="flex col-span-3 items-center justify-center m-2 relative">
                            <img :src="product.images.zoomed[currentIndex].path" class="w-60 h-80 sm:w-64 h-80"
                                @touchstart="touchHandler($event); onTouchStart($event)" @touchmove="onTouchMove"
                                @touchend="onTouchEnd" ref="modalImage" />
                        </div>
                        <div class="p-5 border-4 h-fit">
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
/**
 * This component is parent component for Modal, Tab and Thumbnail component.
 */
export default {
    name: 'ProductImage',
    components: {
        Modal,
        Tab,
        Thumbnail
    },
    data() {
        return {
            /**
             * To track the current index for the media slider.
             */
            currentIndex: 0,
            /**
             * Boolean flag property for the visibility of the modal.
             */
            showModal: false,
            /**
             * Boolean flag property for the on hover magnify functionality for the media files (Desktop version)
             */
            isZoomed: false,
            /**
             * Boolean flag property to track the screen size whether it's mobile or desktop.
             */
            isSmallScreen: false,
            /**
             * Tab data such as heading, ID and visibility flag.
             * Currently this is static but can be made dynamic with dynamic data source e.g. API.
             */
            tabs: [
                { id: "image", label: 'Image', visible: true },
                { id: "video", label: 'Video', visible: true },
            ],
            /**
             * Boolean flag property for the double tap zoom in/out functionality for the media files (Mobile version).
             */
            isZoomIn: false,
            /**
             * Property to hold the start and end co-ordinates of the touch (Mobile version).
             */
            touch: {
                start: 0,
                end: 0
            },
            /**
             * Property to track the time difference between the two taps to determine whether it is a valid double tap or not (Mobile version).
             */
            tapTimer: null,
            /**
             * Boolean flag to track whether the user is in double tap zoom mode or not as to handle swipe feature along with it (Mobile version).
             */
            isPopupZoomMode: false
        }
    },
    computed: {
        /**
         * Computed value to count number of images (not of videos).
         */
        getImageArrayMaxLength() {
            var counter = 0;
            this.product.images.main.map((item) => {
                if (!item.isVideo) counter++;
            });
            return counter;
        },
        /**
         * Computed value to count number of images of product videos.
         */
        getVideoArrayMaxLength() {
            var counter = 0;
            this.product.images.main.map((item) => {
                if (item.isVideo) counter++;
            });
            return counter;
        },
        /**
         * Computed value for the text made up of product name and color used in the modal popup.
         */
        productNameWithColorFormat() {
            return this.product.details.name + ' (Color: ' + this.product.details.color + ')';
        }
    },
    methods: {
        /**
         * Method to decreament the current index of the media slider.
         */
        previous() {
            if (this.currentIndex > 0) this.currentIndex--;
        },
        /**
         * Method to increament the current index of the media slider.
         */
        next() {
            var maxLength = 0;
            this.isSmallScreen ? maxLength = this.getImageArrayMaxLength : maxLength = this.product.images.main.length;
            if (this.currentIndex < (maxLength - 1)) this.currentIndex++;
        },
        /**
         * Method to display the modal popup.
         */
        openModal() {
            this.showTabBasedOnScreenSize();
            this.showModal = true;
        },
        /**
         * Method to set the visibility of the videos tab based on the screen size as for smaller screen size videos should not be visible.
         */
        showTabBasedOnScreenSize() {
            this.isSmallScreen ? this.tabs[1].visible = false : this.tabs[1].visible = true;
        },
        /**
         * Method to display the magnified version of the image being hovered (Desktop version).
         */
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
        /**
         * Method to display selected media based on the currentIndex property.
         */
        displaySelectedMedia(index) {
            this.currentIndex = index;
        },
        /**
         * Method to track the screen size.
         */
        screenSizeTracker() {
            this.currentIndex = 0;
            this.isSmallScreen = window.innerWidth < 640;
        },
        /**
         * Method to display the zoom im/out version of the image by double tap (Mobile version).
         */
        touchHandler(event) {
            var self = this;
            var isValidDoubleClick = false;

            // If set time out then it is a single tap else double tap
            if (this.tapTimer == null) {
                this.tapTimer = setTimeout(function () {
                    self.tapTimer = null;
                    isValidDoubleClick = false;
                }, 500)
            } else {
                clearTimeout(this.tapTimer);
                this.tapTimer = null;
                isValidDoubleClick = true;
            }

            if (isValidDoubleClick) {
                const modalImage = this.$refs.modalImage;
                const modalRect = modalImage.getBoundingClientRect();

                const x = event.touches[0].clientX - modalRect.left;
                const y = event.touches[0].clientY - modalRect.top;

                if (this.isPopupZoomMode) {
                    modalImage.style.transformOrigin = 'initial';
                    modalImage.style.transform = `scale(1, 1)`;
                    this.isPopupZoomMode = false;
                } else {
                    modalImage.style.transformOrigin = `${x}px ${y}px`;
                    modalImage.style.transform = `scale(3, 3)`;
                    this.isPopupZoomMode = true;
                }
                modalImage.style.transition = 'transform 0.3s ease';
            }
        },
        /**
         * Method to track touch start event (Mobile version). 
         */
        onTouchStart(event) {
            this.touch.start = event.touches[0].clientX;
            this.touch.end = 0;
        },
        /**
         * Method to track touch move event (Mobile version). 
         */
        onTouchMove(event) {
            this.touch.end = event.touches[0].clientX;
        },
        /**
         * Method to track touch end event (Mobile version). 
         */
        onTouchEnd() {
            if (!this.isPopupZoomMode && Math.abs(this.touch.end - this.touch.start) > 100 && this.touch.end !== 0) {
                if (this.touch.end < this.touch.start) {
                    this.next();
                } else {
                    this.previous();
                }
            }
            this.touch.start = this.touch.end = 0;
        }
    },
    mounted() {
        this.screenSizeTracker();
        /**
         * Resize event binding to track the screen size.
         */
        window.addEventListener('resize', this.screenSizeTracker);
    },
    beforeUnmount() {
        /**
         * Unbinding the resize event.
         */
        window.removeEventListener('resize', this.screenSizeTracker);
    },
    props: {
        /**
         * Prop to accept the product information. Whole object is required as media and textual information both are used in this component.
         */
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
}
</style>