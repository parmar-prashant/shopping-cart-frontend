<template>
    <div class="grid grid-cols-subgrid grid-cols-1 grid-rows-9 p-5">
        <div>
            <span class="uppercase text-sm font-light">{{ detail.brand }}</span>
            <span :v-if="detail.quantity < 3" class="float-end text-teal-500">
                Hurry, Limited Stock
            </span>
        </div>
        <div class="font-bold text-lg">{{ detail.name }}</div>
        <div>{{ detail.currency }} {{ detail.price }}</div>
        <hr class="h-px my-2 bg-gray-200 border-0 bg-gray-400">
        <div>
            <span class="font-bold">Sim: </span>{{ detail.sim }}
        </div>
        <div>
            <span class="font-bold">Condition: </span>{{ detail.condition }}
        </div>
        <div>
            <span class="font-bold">Color: </span>{{ detail.color }}
        </div>
        <div>
            <span class="font-bold">Network: </span>{{ detail.network }}
        </div>
        <div>
            <span class="font-bold">Storage: </span>{{ detail.storage }}
        </div>
        <hr class="h-px my-2 bg-gray-200 border-0 bg-gray-400">
        <div class="w-auto">
            <div class="inline-flex">
                <button class="py-2 mr-2 text-black">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-dash"
                        viewBox="0 0 16 16">
                        <path d="M4 8a.5.5 0 0 1 .5-.5h7a.5.5 0 0 1 0 1h-7A.5.5 0 0 1 4 8z" />
                    </svg>
                </button>
                <input type="text" class="quantity w-12 py-1 text-center border-0 bg-gray-300" placeholder="1" disabled>
                <button class="py-2 ml-2 text-black">
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-plus"
                        viewBox="0 0 16 16">
                        <path
                            d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z" />
                    </svg>
                </button>
            </div>
        </div>
        <div class="mt-1">
            <button class="w-full bg-yellow-300 p-2">
                Add to Basket
            </button>
        </div>
    </div>
</template>
<script>
export default {
    name: 'ProductDetail',
    data() {
        return {

        }
    },
    props: {
        detail: {
            type: Object,
            required: true
        }
    }
}
</script>
<style>
.quantity ::placeholder {
  color: black;
}
</style>