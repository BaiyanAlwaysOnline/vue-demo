<template>
    <div class="list" @scroll="onScroll" ref="list">
        <ul :style="{ height: virtualHeight + 'px', position: 'relative' }">
            <li
                class="list-item"
                v-for="item in renderDispalyContent"
                :key="item.index"
                :style="item.style"
            >
                id: {{ item.index }}
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    data() {
        return {
            // 行高
            rowHeight: 30,
            originStartIdx: 0,
            startIndex: 0,
            endIndex: 0,
            height: 500,
            bufferSize: 0,
            scrollTop: 0,
        };
    },
    props: {
        total: {
            type: Number,
        },
    },
    mounted() {
        this.endIndex = this.startIndex + this.limit;
        this.bufferSize = this.limit;
    },
    computed: {
        // 虚拟列表总高度
        virtualHeight: ({ total, rowHeight }) => {
            return total * rowHeight;
        },
        // 页面可展示列表个数
        limit: ({ height, rowHeight }) => {
            return Math.ceil(height / rowHeight);
        },
        renderDispalyContent: ({ rowHeight, startIndex, endIndex }) => {
            const content = [];
            for (let i = startIndex; i <= endIndex; i++) {
                content.push({
                    index: i,
                    id: Math.random(),
                    style: {
                        width: "505px",
                        height: rowHeight + "px",
                        position: "absolute",
                        left: 0,
                        right: 0,
                        top: i * rowHeight + "px",
                        borderBottom: "1px solid #ccc",
                    },
                });
            }
            return content;
        },
    },
    methods: {
        onScroll(evt) {
            // 判断是否是我们需要响应的滚动事件
            if (evt.target === this.$refs["list"]) {
                const { scrollTop } = evt.target;
                const { rowHeight, limit, total } = this;
                // 计算当前startIndex
                const currentStartIndex = Math.floor(scrollTop / rowHeight);
                if (currentStartIndex !== this.startIndex) {
                    this.originStartIdx = currentStartIndex;
                    // 增加缓冲区 多渲染一些数据，解决快速滑动会出现列表闪烁的现象/来不及渲染、空白的现象；
                    // 即上下多渲染一些元素用来过渡快速滑动时来不及渲染的问题
                    // 对 startIndex 进行 头部 缓冲区 计算
                    this.startIndex = Math.max(
                        this.originStartIdx - this.bufferSize,
                        0
                    );
                    // 对 endIndex 进行 尾部 缓冲区 计算
                    this.endIndex = Math.min(
                        currentStartIndex + limit + this.bufferSize,
                        total
                    );
                }
                this.scrollTop = scrollTop;
            }
        },
    },
};
</script>
<style lang="less" scoped>
.list {
    height: 500px;
    overflow-y: auto;
    li {
        list-style: none;
        height: 30px;
        line-height: 30px;
        text-align: center;
    }
}
</style>