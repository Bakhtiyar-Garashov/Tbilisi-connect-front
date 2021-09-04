<template>
  <div id="filterbyname-page">
    <ul id="suggested-names-list">
      <li class="suggested-name" v-for="each in allFilteredObjects.features" :key="each.properties.id">
        <a href="/" @click.prevent="emitObjectId(each.id)"> <b> {{ each.properties.name | capitalize }}</b> </a>
      </li>
    </ul>

    <button id="hide" @click.prevent="emitHideCommand">Hide</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: ["searchWord"],
  data() {
    return {
      allFilteredObjects: [],
    };
  },
  methods: {
    emitTagText(text) {
      this.$emit("emitTagText", text);
    },
    emitHideCommand() {
      this.$emit("closeFilterByNamePage");
    },

    emitObjectId(id) {
        this.$emit("emitObjectId", id);
    },
  },


  watch: {
    searchWord: async function() {
      try {
        if (this.searchWord.length > 0) {
          const data = await axios.get(
            `https://tbilisi-connect.herokuapp.com/api/v1/restaurants/?name=${this.searchWord}`
          );
          this.allFilteredObjects = data.data;
        } else {
            this.allFilteredObjects = [];
        }
      } catch (error) {
        alert(`Error happened: ${error}`);
      }
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
#filterbyname-page {
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
  #filterbyname-page {
    position: fixed;
    width: 100%;
  }
}

.title {
  font-size: 12px;
  font-family: "Poppins", sans-serif;
  line-height: 18px;
}

#hide {
  position: absolute;
  right: 40px;
  bottom: 15px;
  font-weight: 500;
}

.suggested-name {
    margin: 25px 0;
    font-size: 16px;
    font-family: "Poppins", sans-serif;
    line-height: 24px;
}
</style>
