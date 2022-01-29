<template>
  <div class="wrapper">
    <header>
      <div class="navbar">
        <div class="container">
          <div class="navbar-content">
            <div class="logo">GitHub-finder</div>
          </div>
        </div>
      </div>
    </header>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      links: [{ title: "Home", url: "/" }],
    };
  },
};
</script>

<style lang="scss">
.navbar-link {
  &.router-link-exact-active {
    color: red;
  }
}
label {
  display: block;
  font-size: 16px;
  margin-bottom: 10px;
  text-align: center;
}
input {
  padding: 16px 26px;
  margin-bottom: 30px;
  border-radius: 20px;
}
textarea {
  padding: 30px 27px;
  border-radius: 14px;
}
input,
textarea {
  width: 100%;
}

//
.btn {
  display: inline-block;
  padding: 0.8em 2em;
  line-height: 1;
  text-transform: uppercase;
  cursor: pointer;
  border-radius: 30px;
  &.btnPrimary {
    color: #ffffff;
    border: 1px solid #494ce8;
    background-color: #494ce8;
    transition: all 0.6s ease;
    &:hover {
      background-color: #ffffff;
      color: #494ce8;
    }
  }
}
</style>
