<template>
  <div class="home">
    <h3>SelectBox组件:</h3>
    <SelectBox :selectTag="fakeTag" :selectOptions="fakeData"></SelectBox>
    <!-- <h3>TimePicker组件:</h3> -->
    <!-- <TimePicker></TimePicker> -->
  </div>
</template>

<script>
import SelectBox from "@/components/select/SelectBox.vue";
import TimePicker from "@/components/time-picker/TimePicker.vue";
export default {
  name: 'Home',
  components: {
    SelectBox,
    TimePicker
  },
  data() {
    return {
      fakeTag: [],
      fakeData: [],
    };
  },
  created() {
    setTimeout(() => {
      this.fakeData.push({
        option: "相机",
        id: "001"
      },
        {
          option: "香蕉",
          id: "002"
        },
        {
          option: "苹果",
          id: "003"
        },
        {
          option: "橘子",
          id: "004"
        },
        {
          option: "葡萄",
          id: "005"
        },
        {
          option: "有道词典",
          id: "006"
        },
        {
          option: "柯南测试",
          id: "007"
        },
        {
          option: "笔记本",
          id: "008"
        },
        {
          option: "火腿肠",
          id: "009"
        },
        {
          option: "新华词典",
          id: "010"
        },
        {
          option: "100号",
          id: "011"
        },
        {
          option: "电脑",
          id: "012"
        })
    }, 1000)
  }
}
</script>
