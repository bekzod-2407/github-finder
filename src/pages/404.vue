<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <h1 class="title">PAGE NOT FOUND</h1>
        <p>GO TO <router-link class="link" to="/"> Main Page</router-link></p>
      </div>
    </section>
  </div>
</template>

<style lang="scss" scoped>
.wrapper-content {
    min-height: 80vh;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
}
</style>