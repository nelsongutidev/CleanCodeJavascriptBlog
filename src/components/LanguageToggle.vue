<template>
  <div class="toggle-switch">
    <button v-bind:class="{'spanish' : (lang === 'Español')}" @click="toggleLanguage">{{lang}}</button>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      lang: "English"
    };
  },
  methods: {
    toggleLanguage: function() {
      this.$emit("clicked");
      if (this.lang === "English") {
        this.lang = "Español";
      } else {
        this.lang = "English";
      }
    }
  }
};
</script>

<style  scoped>
.toggle-switch {
  position: absolute;
  top: 10px;
  left: 10px;
}

button {
  color: #fff;
  text-transform: uppercase;
  text-decoration: none;
  background: rgb(28, 70, 107);
  padding: 12px;
  border-radius: 50px;
  display: inline-block;
  border: 1px solid #fff;
  transition: all 0.4s ease 0s;
}

.spanish {
  background: #ededed;
  border: 1px solid rgb(28, 70, 107);
  color: rgb(28, 70, 107);
}

button:focus {
  outline: 0;
}
</style>


