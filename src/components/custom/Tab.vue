<template>
    <div>
        <div class="tabs">
            <template v-for="tab in tabs" :key="tab.id">
                <button @click="activateTab(tab.id)" :class="{ 'active-tab': activeTab === tab.id }" v-show="tab.visible">
                    {{ tab.label }}
                </button>
            </template>
        </div>

        <div v-for="tab in tabs" :key="tab.id" v-show="activeTab === tab.id" class="tab-content">
            <slot :name="tab.id"></slot>
        </div>
    </div>
</template>
  
<script>
export default {
    name: 'Tab',
    data() {
        return {
            activeTab: null,
        };
    },
    props: {
        tabs: {
            type: Array,
            required: true,
        },
    },
    methods: {
        activateTab(tabId) {
            this.activeTab = tabId;
        },
    },
    created() {
        this.activeTab = this.tabs[0].id;
    },
};
</script>
  
<style>
.tabs {
    display: flex;
}

.tabs button {
    padding: 10px;
    border: none;
    cursor: pointer;
}

.tabs button:hover {
    border-bottom: 2px solid #3498db;
}

.active-tab {
    border-bottom: 2px solid #3498db !important;
}

.tab-content {
    padding: 20px;
}
</style>
  