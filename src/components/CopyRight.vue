<template>
  <div class="copyright">
    <p>
      This game owned by RHP Team in Vue 3 course for begginers -
      <a href="">view here</a>
    </p>
  </div>
</template>
<style lang="css" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}

.copyright a {
  color: #f4dc26;
}
</style>
