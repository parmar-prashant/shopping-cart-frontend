<template>
    <div class="grid grid-cols-2 gap-2">
        <div class="flex place-content-center col-span-2 sm:col-span-1">
            <ProductImage :product="productList[0]" />
        </div>
        <div class="col-span-2 sm:col-span-1">
            <ProductDetail :detail="productList[0].details" />
        </div>
    </div>
</template>

<script>
import ProductImage from "./ProductImage.vue";
import ProductDetail from "./ProductDetail.vue";

export default {
    name: 'Product',
    components: {
        ProductImage,
        ProductDetail
    },
    data() {
        return {
            productList: [{
                images: {
                    main: [
                        {
                            path: '/images/product-image.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/product-image-front.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/product-image-back.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 0,
                            isVideo: true
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 1,
                            isVideo: true
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 2,
                            isVideo: true
                        }
                    ],
                    thumbnail: [
                        {
                            path: '/images/product-image-thumbnail.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/product-image-front-thumbnail.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/product-image-back-thumbnail.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 0,
                            isVideo: true
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 1,
                            isVideo: true
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 2,
                            isVideo: true
                        }
                    ],
                    zoomed: [
                        {
                            path: '/images/product-image-large.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/product-image-front-large.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/product-image-back-large.jpeg',
                            isVideo: false
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 0,
                            isVideo: true
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 1,
                            isVideo: true
                        },
                        {
                            path: '/images/video-thumbnail.png',
                            id: 2,
                            isVideo: true
                        }
                    ],
                },
                videos: [
                    {
                        thumbnail: '/images/video-thumbnail.png',
                        url: '/videos/1.mp4'
                    },
                    {
                        thumbnail: '/images/video-thumbnail.png',
                        url: '/videos/2.mp4'
                    },
                    {
                        thumbnail: '/images/video-thumbnail.png',
                        url: '/videos/3.mp4'
                    }
                ],
                details: {
                    type: 'MOBILE',
                    brand: 'Samsung',
                    name: 'Samsung Galaxy S23 Plus 5G',
                    currency: "£",
                    price: 880.00,
                    quantity: 10,
                    sim: 'Dual Sim',
                    condition: 'Like New',
                    color: 'Lavender',
                    network: 'Unlocked',
                    storage: '512 GB',
                    description: 'With a long-lasting 4,700mAh (typical) battery on Galaxy S23+ and 3,900mAh (typical) on Galaxy S23'
                }
            }],
            currentIndex: 0
        }
    }
}
</script>

<style></style>