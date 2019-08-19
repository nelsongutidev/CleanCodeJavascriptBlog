<template>
  <div class="layout">
    <slot />
    <div class="footer">
      <p>
        Made by
        <a class="link" href="//twitter.com/nel81212">Nelson G</a>
      </p>
    </div>
  </div>
</template>

<style>
body {
  font-family: "Quicksand", sans-serif;
  margin: 0;
  padding: 0;
  line-height: 1.5;
  border: 5px solid rgb(28, 70, 107);
  min-height: 100vh;
}

.layout {
  max-width: 760px;
  margin: 0 auto;
  padding-left: 20px;
  padding-right: 20px;
}

.footer {
  padding-top: calc(30em / 4);
  text-align: center;
  font-size: 1em;
}

.link {
  padding: 7px;
  color: #333;
  text-decoration: underline;
  text-decoration-color: hsl(208, 59%, 26%);
}

.link:hover {
  background-color: hsl(208, 59%, 20%);
  border-radius: 10px;
  color: whitesmoke;
  text-decoration: none;
}
</style>
