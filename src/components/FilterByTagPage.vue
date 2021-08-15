<template>
  <div id="filterbytag-page">
    <div id="title">
      <p class="title">Suggestions</p>
    </div>
    <div id="tags-list">
      <span class="each-tag" :key="tag.id" @click="show(tag.text)" v-for="tag in allTagsList">{{
        tag.text | capitalize
      }}</span>
    </div>
    <button id="hide" @click.prevent="emitHideCommand">Hide</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      allTagsList: [],
    };
  },
  created() {
    axios
      .get("http://127.0.0.1:8000/api/v1/tags/")
      .then((response) => (this.allTagsList = response.data))
      .catch((err) => alert(err));
  },
  methods:{
      show(value){
          alert(value);
      },
      emitHideCommand(){
          this.$emit('emitHideCommand');
      }
  },
  filters: {
    capitalize: function(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
    prettify: function(value) {
      if (!value) return "";
    },
  },
};
</script>

<style scoped>
#filterbytag-page {
  background-color: white;
  width: 500px;
  height: calc(100vh - 140px);
  padding-left: 20px;
  padding-right: 40px;
  position: absolute;
  top: 140px;
  right: 0;
  overflow-y: auto;
  overflow-x: hidden;
}

.title {
  font-size: 12px;
  font-family: "Poppins", sans-serif;
  line-height: 18px;
}

#tags-list {
    width: 100%;
    word-wrap: break-word;
}

.each-tag {
    padding: 1px 4px;
    font-family: "Montserrat", sans-serif;
    font-size: 12px;
    line-height: 14.63px;
    text-align: center;
    border: 1px solid black;
    border-radius: 4px;
    margin-right: 10px;
    white-space: nowrap;
    cursor: pointer;
}

.each-tag:hover {
  background-color: black;
  color: #ff7701;
}

#hide {
  position: absolute;
  right: 40px;
  bottom: 15px;
  font-weight: 500;
}
</style>

