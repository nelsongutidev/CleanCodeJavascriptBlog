<template>
  <div v-if="language === 'en'">
    <p>
      A guide to producing readable, reusable, and refactorable
      software in JavaScript by
      <a
        class="link"
        href="https://twitter.com/ryconoclast"
      >Ryan McDermott</a>
    </p>
    <p>
      I am by all means
      <span>not</span> the author of any of these posts. Instead,
      I am a JS developer who has found HUGE value in them.
      I have created this site to make the information more accesible to more people and have it in blog form.
    </p>
  </div>
  <div v-else>
    <p>
      Esto es una guía
      para crear código legible, reutilizable y de fácil modificación en Javascript. Originalmente escrito por
      <a
        class="link"
        href="https://twitter.com/ryconoclast"
      >Ryan McDermott</a> y traducido por
      <a class="link" href="https://www.elrincondevictor.com/">Victor Ribero,</a>los siguientes son 11 posts
      sobre Clean Code JS.
    </p>
    <p>
      Debo aclarar que
      <span>no</span> soy el autor de ninguno de estos posts. Mas bien,
      soy un desarrollador de JS que ha encontrado enorme valor en ellos y ha querido crear un sitio
      para difundir mas y mas estas ideas.
    </p>
  </div>
</template>

<script>
export default {
  props: {
    language: String
  }
};
</script>