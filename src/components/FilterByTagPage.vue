<template>
  <div id="filterbytag-page">
    <div id="title">
      <p class="title">Suggestions</p>
    </div>
    <div id="tags-list">
      <span
        class="each-tag"
        :key="tag.id"
        @click.prevent="emitTagText(tag.text)"
        v-for="tag in allTagsList"
        ><img :src="iconBtc" class="icon-btc" v-if="tag.text.toLowerCase()==='cryptocurrency'"/>{{ tag.text | capitalize }}</span
      >
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
      iconBtc: require('../assets/btc.svg'),
    };
  },
  created() {
    axios
      .get("https://tbilisi-connect.herokuapp.com/api/v1/tags/")
      .then((response) => (this.allTagsList = response.data))
      .catch((err) => alert(err));
  },
  methods: {
    emitTagText(text){
      this.$emit('emitTagText', text);
    },
    emitHideCommand() {
      this.$emit("emitHideCommand");
    },
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
  z-index: 2;
  overflow-y: auto;
  overflow-x: hidden;
}

@media only screen and (max-width: 500px) {
  #filterbytag-page {
    position: fixed;
    width: 100%;
  }
}

.title {
  font-size: 12px;
  font-family: "Poppins", sans-serif;
  line-height: 18px;
}

#tags-list {
  width: 100%;
}

.each-tag {
  display: inline-block;
  align-items: center;
  justify-content: center;
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

.each-tag img {
  display: inline;
  margin-bottom: 1px;
}

#hide {
  position: absolute;
  right: 40px;
  bottom: 15px;
  font-weight: 500;
}
</style>
