<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <!-- <form>
      <input type="text" placeholder="name" v-model="dataForm.name" />
      <button @click="onSubmit">save</button>
    </form> -->

    <el-dialog title="Tips" :visible.sync="dialogVisible" width="30%">
      <el-tag
        :key="tag"
        v-for="tag in dynamicTags"
        closable
        :disable-transitions="false"
        @close="handleCloseTag(tag)"
      >
        {{ tag }}
      </el-tag>
      <el-input
        class="input-new-tag"
        v-if="inputVisible"
        v-model="dataForm.name"
        ref="saveTagInput"
        size="mini"
        @keyup.enter.native="handleInputConfirm"
      >
      </el-input>
      <el-button v-else class="button-new-tag" size="small" @click="showInput"
        >+ New Tag</el-button
      >

      <span slot="footer" class="dialog-footer">
        <el-button @click="handleClose">Cancel</el-button>
        <el-button type="primary" @click="onSubmit">Save</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
const dataForm = {
  id: undefined,
  name: null,
};
export default {
  name: "HelloWorld",
  props: {
    msg: String,
    data: {
      type: Array,
      required: false,
      default: () => [],
    },
    dialogVisible: Boolean,
  },
  data() {
    return {
      dynamicTags: [],
      inputVisible: false,
      dataForm: { ...dataForm },
    };
  },
  methods: {
    handleClose() {
      let check = false;
      this.$emit("handleClose", check);
    },
    handleCloseTag(tag) {
      this.dynamicTags.splice(this.dynamicTags.indexOf(tag), 1);
    },

    showInput() {
      this.inputVisible = true;
      this.$nextTick(() => {
        this.$refs.saveTagInput.$refs.input.focus();
      });
    },

    handleInputConfirm() {
      let inputValue = this.dataForm.name;
      let isDuplicate = false;
      let checkDuplicateData = this.data.filter(
        (dataItem) => inputValue === dataItem.name
      );
      let checkDuplicateInputValue = this.dynamicTags.filter(
        (tag) => inputValue === tag
      );
      if (
        checkDuplicateData.length > 0 ||
        checkDuplicateInputValue.length > 0
      ) {
        isDuplicate = true;
      }
      if (inputValue && !isDuplicate) {
        this.dynamicTags.push(inputValue);
      } else {
        return alert("duplicate inputvalue");
      }
      this.inputVisible = false;
      this.dataForm.name = null;
      this.inputValue = null;
    },

    onSubmit(e) {
      e.preventDefault();
      const id = this.data[this.data.length - 1].id + 1;
      // console.log({
      //   id: id,
      //   name: this.dataForm.name
      // })
      this.dataForm.id = id;
      this.$emit("addData", this.dynamicTags);
      this.dataForm = { ...dataForm };
      this.dynamicTags = [];
      this.dataForm.name = null;
      this.handleClose();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
