<template>
  <div class="header">
    <div class="logo">
      <img src="../assets/todo-logo.png" alt="">
    </div>
    <div class="search">
      <input type="text">
      <button class="btn"> Найти</button>
    </div>
    <div class="github-link">
      <div class="github-link__moon">
        <img class='moon' src="../assets/moon.png" alt="">
        <img class="github-logo" src="../assets/github-logo.png" alt="">
      </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style lang='sass'>
.header
  display: flex
  flex-direction: row
  justify-content: space-between
  align-items: flex-start
  width: 100vw
  height: 15vh
  padding: 0rem 2rem
  padding-top: 1rem
  box-sizing: border-box
  background: linear-gradient(0deg, #1f3a47, darken(#1f3a47, 7%))
  overflow: hidden
// Logo style
.logo
  flex-basis: 20%
  height: 100%
  background-color: transparent
  & img
    width: 12rem
    margin-top: -1.8rem

// Search-panel style
.search
  display: flex
  flex-direction: row
  align-items: center
  flex-wrap: nowrap
  & input
    padding: 0.5rem
    margin: 0
    margin-right: 0.5rem
    background-color:
    width: 20rem
    border: 2px solid white
    border-radius: 0.25rem
.btn
  width: 5rem
  padding: 0.4rem 0.25rem
  color: #1f3a47
  background-color: #b7c4c8
  border: none
  border-radius: 0.25rem
// GitHub Link Style
.github-link
  flex-basis: 20%
  height: 100%
  background-color: transparent
  &__moon
    position: absolute
    overflow: hidden
    width: 20vw
    height: 80vh
    top: -12rem
    right: 0
.github-logo
  cursor: pointer
  position: absolute
  top: 13rem
  width: 10rem
  right: 2rem
.moon
  z-index: 99
  right: -12rem

</style>
