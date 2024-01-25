<template>
    <template v-for="(image, index) in thumbnails" :key="image">
        <template v-if="contentType == 'all'">
            <img :src="image.path" :class="[thumbnailDesignClass, { 'border-blue-800': index == currentIndex }]"
                @click="$emit('syncCurrentIndex', index)" />
        </template>
        <template v-if="contentType == 'image'">
            <img :src="image.path" :class="[thumbnailDesignClass, { 'border-blue-800': index == currentIndex }]"
                @click="$emit('syncCurrentIndex', index)" v-if="!image.isVideo" />
        </template>
        <template v-if="contentType == 'video'">
            <img :src="image.path" :class="[thumbnailDesignClass, { 'border-blue-800': index == currentIndex }]"
                @click="$emit('syncCurrentIndex', index)" v-if="image.isVideo" />
        </template>

    </template>
</template>
<script>
export default {
    name: 'Thumbnail',
    data() {
        return {
            thumbnailDesignClass: 'border border-1 p-3 m-1 inline-block h-15 w-15',
        }
    },
    emits: ['syncCurrentIndex'],
    props: {
        thumbnails: {
            type: Object,
            required: true
        },
        contentType: {
            type: String,
            required: true
        },
        currentIndex: {
            type: Number,
            required: true
        }
    }
}
</script>
<style></style>