<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <!-- 多选,单选,框选 -->
    <h1>实现多选,单选,框选</h1>
    <div class="tool-box">
      <div class="btn" @click="mood = !mood">
        {{ mood ? "点击变宽" : "点击收缩" }}
      </div>
      <div class="btn" @click="insertItem">添加一个柴犬</div>
      <div class="tips">
        可以尝试
        <code>Ctrl</code>、 <code>command</code> 和 <code>shift</code> 按键
      </div>
      <div style="margin-top: 20px">
        <code>已选中</code>:{{ selectedList.length }}个柴犬啦
      </div>
    </div>
    <div :class="['index-page', mood ? '' : 'bigger']">
      <div class="result-box">
        <vue-drag-select
          v-model="selectedList"
          value-key="name"
          :item-margin="[0, 10, 10, 0]"
          ref="dragSelect"
        >
          <template v-for="(item, index) in dataList">
            <drag-select-option
              :key="item.id"
              :value="item"
              :item-index="index"
            >
              <div class="item-self">
                <img v-if="index === 100" src="./assets/images/prank.jpg" />
                <img v-else-if="item.lip" src="./assets/images/1.jpg" />
                <img v-else src="./assets/images/0.jpg" />
              </div>
            </drag-select-option>
          </template>
        </vue-drag-select>
      </div>
    </div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
// 大盒子
import vueDragSelect from './components/vue-drag-select.vue'
// 小盒子
import dragSelectOption from './components/drag-select-option'

export default {
  name: 'App',
  components: {
    vueDragSelect,
    dragSelectOption
  },
  data () {
    return {
      mood: true,
      dataList: [
        {
          id: 1
        },
        {
          id: 2
        },
        {
          id: 3
        }
      ],
      selectedList: [],
      id: 300
    };
  },
  created () {
    // 准备数据
    this.dataList = [];
    for (let i = 50; i <= 300; i++) {
      this.dataList.push({
        id: i,
        name: i
      });
    }
  },
  watch: {
    mood () {
      clearTimeout(this.timeClick);
      this.timeClick = setTimeout(() => {
        // 元素布局函数
        this.$refs.dragSelect.elementLayout(200, 230);
      }, 500);
    },
    selectedList: {
      deeply: true,
      handler (newValue) {
        console.log('newvalue', newValue)
      }
    }
  },

  methods: {
    insertItem () {
      const { id, name } = this.dataList.reduce((p, v) =>
        p.id < v.id ? v : p
      );
      // 向数组中添加新元素
      this.dataList.splice(0, 0, {
        id: id + 1,
        name: name + 1,
        lip: true
      });
    }
  }
}
</script>

<style lang='less'>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.tool-box {
  // position: absolute;
  // left: 10px;
  // top: 50px;
  // height: 100%;
  // display: flex;
  // flex-direction: column;
  // justify-content: center;
  .btn {
    width: 20%;
    margin: 0 auto;
    text-align: center;
    margin-bottom: 20px;
    padding: 10px 20px;
    color: #fff;
    background-color: #2d8cf0;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s ease;
    user-select: none;
    &:hover {
      background-color: #57a3f3;
    }
    &:focus {
      box-shadow: 0 0 0 2px rgba(45, 140, 240, 0.2);
    }
    &:active {
      background-color: #2d8cf0;
    }
  }
  .tips {
    code {
      background: #000;
      color: #fff;
      padding: 10px;
      border-radius: 6px;
    }
  }
}
.index-page {
  width: 70%;
  height: 100%;
  position: relative;
  // padding: 0 0 0 400px;
  margin: 0 auto;
  transition: all 0.3s ease;

  .result-box {
    width: 100%;
    height: 500px;
    padding-top: 50px;
    transition: all 0.3s;
    .vue-drag-select {
      margin: 0 auto;
      background-color: #ddd;
    }
    .item-self {
      width: 100%;
      height: 100%;
      border-radius: 4px;
      border: 1px solid #fff;
      background-color: #fff;
      transition: all 0.3s ease;
      overflow: hidden;
      &:hover {
        box-shadow: 0px 2px 20px -2px rgba(0, 0, 0, 0.5);
      }
      img {
        width: 100%;
      }
    }
    .selected-item {
      .item-self {
        border: 1px solid red;
        border-color: rgb(65, 98, 255);
        box-shadow: rgb(65, 98, 255) 0px 0px 0px 2px !important;
      }
    }
  }
}
.bigger {
  width: 100%;
  // padding: 0 0 0 100px;
  .result-box {
    height: 600px;
  }
}
</style>
