<template>
    <div ref="wrapper">
        <div :key="tableKey">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    import sortable from "sortablejs";

    export default {
        name: "DragTable",
        props: {
            handle: {
                type: String,
                default: ""
            },
            animate: {
                type: Number,
                default: 100
            }
        },
        data() {
            return {
                tableKey: 0
            };
        },
        methods: {
            makeTableSortAble() {
                let table = this.$children[0].$el.querySelector("#drag-index tbody");
                if (table) {
                    sortable.create(table, {
                        handle: this.handle,
                        animation: this.animate,
                        onStart: () => {
                            this.$emit("drag");
                        },
                        onEnd: ({newIndex, oldIndex}) => {
                            this.keepWrapperHeight(true);
                            this.tableKey = Math.random();
                            const arr = this.$children[0]._data.localItems;
                            if (arr.length > 0) {
                                const firstRow = arr[oldIndex];  //arr.splice(oldIndex, 1)[0];
                                const secondRow = arr[newIndex];  //arr.splice(newIndex, 1)[0];
                                this.$emit("drop", {first: firstRow, second: secondRow});
                            }
                        }
                    });
                }
            },
            keepWrapperHeight(keep) {
                // eslint-disable-next-line prefer-destructuring
                const wrapper = this.$refs.wrapper;
                if (keep) {
                    wrapper.style.minHeight = `${wrapper.clientHeight}px`;
                } else {
                    wrapper.style.minHeight = "auto";
                }
            }
        },
        mounted() {
            this.makeTableSortAble();
        },
        watch: {
            tableKey() {
                this.$nextTick(() => {
                    this.makeTableSortAble();
                    this.keepWrapperHeight(false);
                });
            }
        }
    };
</script>
