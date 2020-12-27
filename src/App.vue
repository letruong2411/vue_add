<template>
  <div id="app">
    <hello-world
      :data="data"
      v-on:addData="addData"
      :dialogVisible="dialogVisible"
      v-on:handleClose="handleClose"
    />

    <table style="border: 1px solid black">
      <tr>
        <td style="width:100px,border: 1px solid black ">id</td>
        <td style="width: 200px">name</td>
        <td style="width: 200px">Action</td>
      </tr>
      <tr v-for="item in data" :key="item.id">
        <td>{{ item.id }}</td>
        <td>{{ item.name }}</td>
        <td>
          <button>edit</button>
          <button>delete</button>
        </td>
      </tr>
    </table>
    <el-button type="text" @click="openDialog">Add</el-button>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
// import EditWords from "./components/EditWords";
import indexData from "./config/indexData";
import "element-ui/lib/theme-chalk/index.css";

export default {
  name: "App",
  components: {
    HelloWorld,
    // EditWords,
  },
  data() {
    return {
      dialogVisible: false,
      data: indexData,
      dialog: false,
    };
  },
  methods: {
    openDialog() {
      this.dialogVisible = true;
    },
    handleClose(check) {
      this.dialogVisible = check;
    },
    // onEdit() {
    //   this.dialogEditVisible = true;
    // },
    addData(dataChild) {
      let listData = dataChild.map((dataItem, index) => {
        let id = this.data.length + 1 + index;
        return { id, name: dataItem };
      });
      if (listData.length > 0) {
        this.data.push(...listData);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
