<template>
  <div id="sidebar">
    <div id="upper">
      <div id="logo">
        <img src="../assets/logo.svg" alt="" srcset="" />
      </div>
      <div id="widgets">
        <div id="about">
          <a href="/">About</a>
        </div>
        <div class="sl-nav">
          <ul>
            <li>
              <span :v-model="selectedLanguage">{{ selectedLanguage }}</span>
              <ul>
                <li>
                  <span class="active">GE</span>
                </li>
                <li>
                  <span>RU</span>
                </li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div id="lower">
      <div v-if="!isSearchActivated" id="right">
        <a href="/" @click.prevent="isSearchActivated = !isSearchActivated"
          >Search</a
        >
      </div>
      <div v-if="isSearchActivated" class="search-container">
        <input
          id="search-field"
          placeholder="What places are you interested in?"
        />
        <span @click="show" class="input-widget">Clear</span>
        <span @click="show" class="input-widget">Filter</span>
      </div>
    </div>
    <slot name="welcome-page"></slot>
    <slot name="restaurant-detail-page"></slot>
    <slot name="restaurant-search-by-tag-page"></slot>
    <slot name="restaurant-search-by-name-page"></slot>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      selectedLanguage: "EN",
      isSearchActivated: false,
    };
  },
  computed: {},
  methods: {
    show(){
      alert('test')
    }
  },
};
</script>

<style scoped>
#sidebar {
  display: flex;
  flex-direction: column;
  width: 500px;
  height: 140px;
  background-color: white;
  position: absolute;
  top: 0%;
  right: 0%;
  z-index: 999;
}

#upper {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  float: right;
}

#lower {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
}

#right {
  margin-right: 39px;
  float: right;
}
#logo img {
  margin-top: 16px;
  margin-left: 20px;
}

#about {
  margin-right: 12px;
}

#about a {
  text-decoration: none;
  font-size: 12px;
  font-weight: 400;
  line-height: 18px;
  color: #1c1c1c;
}
#widgets {
  display: flex;
  align-items: flex-start;
  margin-top: 16px;
  flex-direction: row;
  float: right;
}
.sl-nav {
  display: inline;
  margin-right: 40px;
}
.sl-nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
  position: relative;
  display: inline-block;
}
.sl-nav li {
  cursor: pointer;
  padding-bottom: 10px;
  text-decoration: underline;
}
.sl-nav li ul {
  display: none;
}
.sl-nav li:hover ul {
  position: absolute;
  top: 29px;
  right: -13px;
  display: block;
  background: #fff;
  width: 120px;
  padding-top: 0px;
  z-index: 1;
  border-radius: 5px;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.2);
}

.sl-nav li ul li {
  position: relative;
  text-align: left;
  background: transparent;
  padding: 15px 15px;
  padding-bottom: 0;
  z-index: 2;
  font-size: 12px;
  line-height: 18px;
  color: #1c1c1c;
}
.sl-nav li ul li:last-of-type {
  padding-bottom: 15px;
}
.sl-nav li ul li span {
  padding-left: 5px;
}
.sl-nav li ul li span:hover,
.sl-nav li ul li span.active {
  color: #146c78;
}

span,
a {
  font-size: 12px;
  font-weight: 400;
  line-height: 18px;
  color: black;
}

.search-container {
  width: 100%;
  margin-left: 20px;
  margin-right: 40px;
}

#search-field {
  border: 1px solid #1c1c1c;
  border-radius: 4px;
  width: 100%;
  box-sizing: border-box;
  margin-top: 15px;
  height: 32px;
  padding: 7px 10px;
  font-family: "Poppins", sans-serif;
  font-size: 12px;
  color: #1c1c1c;
}

.input-widget {
  color: black;
  font-size: 12px;
  font-weight: 400;
  position: absolute;
  top: 103px;
  right: 48px;
  cursor: pointer;
}
</style>
