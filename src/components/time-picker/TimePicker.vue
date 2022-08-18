<template>
    <div class="time-picker">
        <!-- input -->
        <div class="time-input">
            <!-- start -->
            <span>
                <i class="el-input__icon el-range__icon el-icon-time"></i>
            </span>
            <input v-model="startDate" placeholder="请选择开始时间">
            <!-- end -->
            <!-- <button class="confirms-btn">确定</button> -->
        </div>
        <!-- options -->
        <div class="time-options">
            <!-- month -->
            <ul class="time-month">
                <li v-for="item in monthData" :key="item.value">
                    <span>{{ item.value }}</span>
                </li>
            </ul>
            <!-- day -->
            <ul class="time-day">
                <li v-for="item in monthData" :key="item.value">
                    <span>{{ item.value }}</span>
                </li>
            </ul>
            <!-- hour -->
            <ul class="time-hour">
                <li v-for="item in monthData" :key="item.value">
                    <span>{{ item.value }}</span>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
export default {
    name: "TimePicker",
    data() {
        return {
            startDate: "8:00:00",
            monthData: [
                { value: 1 },
                { value: 2 },
                { value: 3 },
                { value: 4 },
                { value: 5 },
                { value: 6 },
                { value: 7 },
                { value: 8 },
                { value: 9 },
                { value: 10 },
                { value: 11 },
                { value: 12 },
                { value: 13 },
                { value: 14 },
                { value: 15 },
            ]
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

.time-picker {
    max-width: 400px;
    display: flex;
    flex-direction: column;
    color: #606165;

    // input
    .time-input {
        width: 100%;
        height: 100%;
        position: relative;
        display: flex;
        align-items: center;
        border: 1px solid #e3e3e3;
        border-radius: 4px;
        justify-content: space-between;

        input {
            width: 100%;
            height: 40px;
            border: none;
            outline: none;
            color: #606165;
        }
    }

    // options
    .time-options {
        display: flex;
        border: 1px solid #e3e3e3;
        margin: 8px 0;
        border-radius: 4px;

        .time-month,
        .time-day,
        .time-hour {
            height: 200px;
            background-color: #fff;
            padding: 5px 0;
            width: 50px;
            cursor: pointer;
            overflow: hidden;
            margin: 0 5px;

            li {
                width: 100%;
                color: #606060;
                text-align: center;
                margin: 10px 0;
                // background-color: #e6f7ff;
            }
        }

        // ul_scroll
        ul:hover {
            overflow-y: scroll;

        }
    }
}
</style>