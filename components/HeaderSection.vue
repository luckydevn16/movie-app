<template>
  <header class="header">
      <img src="../assets/img/header.webp" alt="">
      <div class="text-container">
          <div class="text">
              <span class="mini-heading">Now Streaming</span>
              <h1>
                  <span>Movies</span> to Watch
              </h1>
          </div>
          <a href="#movie-grid" class="button">View</a>
      </div>
  </header>
</template>

<script>
export default {
    name: 'HeaderSection'
}
</script>

<style lang="scss" scoped>
.header {
  height: 6rem;
  position: relative;
  @media (min-width: 750px) {
    height: 8rem;
  }
  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
  }
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .text-container {
    z-index: 33;
    position: absolute;
    top: 0;
    width: 90%;
    max-width: 1400px;
    left: 50%;
    transform: translateX(-50%);
    height: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .mini-heading {
      font-weight: 600;
      font-size: .9rem;
      text-transform: uppercase;
      color: #FC427B;
      margin-bottom: .5rem;
      text-shadow: 0 3px 6px #111;
    }
    h1 {
      color: #fff;
      font-size: 1.6rem;
      font-weight: 200;
      margin-bottom: 8px;
      @media (min-width: 750px) {
        font-size: 2.2rem;
      }
      span {
        font-weight: 500;
      }
    }
    .button {
      font-size: .9rem;
    }
  }
}
</style>