<template>
    <div class="container" ref="container">
        <a-modal
            :visible="visible"
            @cancel="visible = false"
            :maskClosable='false'
            :closable='false'
            :bodyStyle='{
                padding: 0
            }'
        >
            <list :total='1000000' />
        </a-modal>
        <button @click="show">show</button>
    </div>
</template>
<script>
import List from "@/components/List.vue";
export default {
    data() {
        return {
            visible: false,
        };
    },
    components: {
        List,
    },
    methods: {
        show() {
            this.visible = true;
        }
    },
};
</script>
<style lang="less" scoped>
.container {
    width: 400px;
    height: 500px;
    overflow-y: auto;
    color: #444;
}
</style>
