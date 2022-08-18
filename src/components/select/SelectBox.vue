<template>
    <div class="select-box">
        <!-- dropdown -->
        <div class="select-dropdown" :class="{ 'dropdownFocus': isFocus }">
            <!-- tag -->
            <div class="select-tags" v-if="selectTag.length > 0">
                <div class="select-tag-item">
                    <span class="text-ellipsis"> {{ selectTag[0] }}</span>
                    <span @click="del()">
                        <i class="el-icon-close"></i>
                    </span>
                </div>
                <span class="select-tag-item" v-if="selectTag.length > 1">+{{ selectTag.length
                        - 1
                }}</span>
            </div>
            <!-- input -->
            <div class="select-input">
                <input v-model="searchValue" ref="input" @focus="handleFocus()" @input="searchEvent($event)"
                    @click.stop="visiable != visiable">
                <span>
                    <i class="el-select__caret el-input__icon el-icon-arrow-up" :class="{ 'reverse': isFocus }">
                    </i>
                </span>
                <!-- <button class="confirms-btn">确定</button> -->
            </div>
        </div>
        <!-- options -->
        <div class="select-options" v-show="visiable" @click.stop>
            <!-- tag -->
            <div class="select-tags">
                <div class="select-tag-item" v-for="(item, index) in selectTag" :key="index">
                    <span class="text-ellipsis">{{ item }}</span>
                    <span @click="del()">
                        <i class="el-icon-close"></i>
                    </span>
                </div>
            </div>
            <!-- option -->
            <div class="select-option-box">
                <div class="select-option-item" v-for="item in selectOptions" :key="item.id"
                    @click="addClick($event, item.option)">
                    <span class="text-ellipsis">{{ item.option }}</span>
                </div>
            </div>
            <!-- pagination -->
            <div class="pagination">
                <!-- <span>第{{ currentPage }}页</span> -->
                <span class="page-li">
                    <button class="prev" @click="prev()" :disabled="isPrevDisabled">
                        <i class="el-icon el-icon-arrow-left"></i>
                    </button>
                </span>
                <span class="page-li" v-for="(item, index) in pages" :key="index"
                    :class="{ active: item === currentPage }" @click="selected(item)">
                    {{ item }}
                </span>
                <span class="page-li">
                    <button class="next" @click="next()" :disabled="isNextDisabled">
                        <i class="el-icon el-icon-arrow-right"></i>
                    </button>
                </span>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: "Button",
    props: {
        selectTag: {
            type: Array,
            required: true
        },
        selectOptions: {
            type: Array,
            required: true
        },
    },
    data() {
        return {
            // v-show
            visiable: false,
            isFocus: false,
            searchValue: "",
            currentPage: 1, //当前页
            totalPages: 10, //总页面
            isPrevDisabled: false,
            isNextDisabled: false,
            num: 1
        };
    },
    created() {
        console.log(this.selectTag,"tag");
        console.log(this.selectOptions,"options");
    },
    mounted() {
        document.addEventListener("click", (event) => { this.cancelPop(event) })
    },
    beforeDestroy() {
        document.removeEventListener("click", () => { }, true)
    },
    methods: {
        handleClick(e) {
            console.log(e);
        },
        // 获取焦点
        handleFocus(e) {
            this.$nextTick(() => {
                let length = this.searchValue.length
                this.$refs.input.focus()
                this.$refs.selectionStart = length
                this.$refs.selectionEnd = length
            });
            this.visiable = true
            this.isFocus = true
        },
        // 关闭弹窗
        cancelPop(event) {
            if (event.target.className !== "select-optios") {
                this.visiable = false
                this.isFocus = false
            }
        },
        // 搜索事件
        searchEvent(e) {
            console.log("搜索后", this.selectOptions);
        },
        // 添加事件
        addClick(event, value) {
            this.selectTag.indexOf(value) != -1 ? "" : this.selectTag.push(value)
        },
        // 删除事件
        del() {
            this.selectTag.pop()
        },
        // 根据index下标-点击选中切换active类名
        selected(n) {
            console.log(this.currentPage);
            if (n === this.currentPage) {
                return
            } else if (typeof n === 'string') {
                return
            } else {
                this.currentPage = n;
            }
            if (this.currentPage < this.totalPages + 1) {
                // 当前页面小于总页面时,Next按钮为可用状态 ...占一个页面,所以判断条件+1
                this.isNextDisabled = false;
            }
            if (this.currentPage > this.currentPage - 1) {
                this.isPrevDisabled = false;
            }
        },
        // 上一页
        prev() {
            console.log('当前页' + this.currentPage);
            // 当前页面小于等于1时,prev按钮为禁用状态
            if (this.currentPage <= 1) {
                this.isPrevDisabled = true;
            } else {
                // 当前页面大于1时,prev按钮为可用状态,执行--操作
                this.currentPage--;
            }
            if (this.currentPage < this.totalPages + 1) {
                // 当前页面小于总页面时,Next按钮为可用状态 ...占一个页面,所以判断条件+1
                this.isNextDisabled = false;
            }
        },
        // 下一页
        next() {
            console.log('当前页' + this.currentPage);
            if (this.currentPage > this.totalPages - 1) {
                // 当前页面大于总页面时,Next按钮为禁用状态 ...占一个页面,所以判断条件-1
                this.isNextDisabled = true;
            } else {
                // 当前页面小于总页面时,Next按钮为可用状态 ...占一个页面,所以判断条件-1
                this.currentPage++;
            }
            // 当前页面大于1时,prev按钮为可用状态 ...占一个页面,所以判断条件-1
            if (this.currentPage > this.currentPage - 1) {
                this.isPrevDisabled = false;
            }
        }
    },
    computed: {
        pages() {
            const c = this.currentPage; //当前页
            const t = this.totalPages; //总页面
            //  当前页小于5时的排序
            if (c <= 5) {
                return [1, 2, 3, 4, 5, 6, '...', t]
            } else if (c >= t - 4) {
                return [1, '...', t - 5, t - 4, t - 3, t - 2, t - 1, t]
            } else {
                return [1, '...', c - 3, c - 2, c - 1, c, '...', t]
            }
        }
    },
    watch: {
        selectOptions(newValue, oldValue) {
            console.log(newValue, oldValue);
        }
    }
}
</script>
<style lang="less" scoped>
ul,
* {
    margin: 0;
    padding: 0;
    list-style: none;
}

// select-box
.select-box {
    max-width: 400px;
    font-size: 14px;

    .select-dropdown {
        border: 1px solid #e3e3e3;
        border-radius: 4px;
    }

    .select-options {
        border-radius: 4px;
        margin-top: 10px;
        border: 1px solid #e3e3e3;
    }
}

// select-dropdown
.select-dropdown {
    // position: relative;
    display: flex;
    flex-wrap: nowrap;
    flex: auto;
    max-width: 100%;
    height: 40px;
    align-items: center;

    // tags
    .select-tags {
        // position: relative;
        // z-index: 1;
        display: flex;
        height: 100%;
        align-items: center;

        .select-tag-item {
            display: flex;
            align-items: center;
            max-width: 100%;
            padding: 4px 15px;
            background-color: #f0f1f2;
            // border: 1px solid #ccc;
            color: #909399;
            border-radius: 4px;
            margin-left: 4px;

            .text-ellipsis {
                flex: 1;
                white-space: nowrap;
                overflow: hidden;
                color: #606060;
                font-size: 14px;
            }
        }

    }

    // input
    .select-input {
        width: 100%;
        height: 100%;
        position: relative;
        display: flex;
        align-items: center;
        padding: 0 5px;

        input {
            width: 100%;
            height: 100%;
            text-indent: 8px;
            border: none;
            outline: none;
        }

        .confirms-btn {
            border: none;
            padding: 5px 8px;
            // border: 1px solid #e3e3e3;
            border-radius: 4px;
            color: #606060;
        }
    }
}

// select-options
.select-options {
    position: relative;
    width: 100%;
    border: 1px solid #e4e7ed;
    box-shadow: 0px 2px 12px rgba(0, 0, 0, 0.1);

    // tags
    .select-tags {
        margin: 5px 0;
        width: 100%;
        display: grid;
        grid-template-columns: repeat(4, 1fr);

        .select-tag-item {
            display: flex;
            align-items: center;
            justify-content: center;
            max-width: 100%;
            padding: 4px 15px;
            background-color: #ecf4fe;
            // border: 1px solid #ccc;
            border-radius: 4px;
            margin: 2px 4px;

            .text-ellipsis {
                flex: 1;
                width: 0;
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
                color: #409EFF;
                font-size: 14px;
            }
        }
    }

    // option
    .select-option-box {
        width: 100%;
        display: grid;
        grid-template-columns: repeat(4, 1fr);

        .select-option-item {
            height: 40px;
            line-height: 40px;
            display: flex;
            border-top: 1px solid #e3e3e3;
            border-right: 1px solid #e3e3e3;

            .text-ellipsis {
                flex: 1;
                width: 0;
                font-size: 14px;
                color: #606165;
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
                cursor: pointer;
                text-align: center;
            }
        }

        .select-option-item:nth-of-type(4n) {
            border-right: none;
        }
    }
}

// pagination
.pagination {
    width: 100%;
    height: 100%;
    border-top: 1px solid #e3e3e3;
    padding: 10px 0;
    display: flex;

    .page-li,
    button {
        width: 32px;
        height: 30px;
        display: flex;
        align-items: center;
        justify-content: center;
        border: none;
        border-radius: 2px;
        font-weight: 550;
        color: #555;
        background-color: #f4f4f5;
        margin: 0 5px;
        cursor: pointer;
    }

    .pali:hover {
        color: rgb(59, 155, 251);
    }

    /* 选中状态 */
    .pagination>.active {
        color: #fff;
        background-color: rgb(59, 155, 251);
        border-radius: 2px;

    }

    /* 默认样式 */
    .prev,
    .next {
        background-color: #f4f4f5;
        color: #939393;
    }

    /* 禁用状态的样式 */
    .prev:disabled,
    .next:disabled {
        cursor: not-allowed;
    }
}

// icon
i {
    cursor: pointer;
}

.reverse {
    transform: rotate(180deg);
}

.el-icon-close {
    // color: #909399;
    color: #606165;
    transform: scale(0.9);
}

.select-box .dropdownFocus {
    border: 1px solid #409EFF;
}
</style>