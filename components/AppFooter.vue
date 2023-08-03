<template>
  <footer class="footer">
      <div class="container">
          <div class="footer-block">
                Data provided by the 
                <a href="https://www.themoviedb.org/" target="_blank" class="footer-link">
                    <img src="../assets/img/tmdb.svg" alt="tmdb" class="footer-link-img">
                </a>
          </div>
          <div class="footer-block">
                Made by 
                <a href="https://redhatdev16.github.io/portfolio/" target="_blank" class="footer-link">
                    redhatdev16
                </a>
          </div>
      </div>
  </footer>
</template>

<script>
export default {
    name: 'AppFooter'
}
</script>

<style lang="scss">
.footer {
    width: 100%;
    padding: 2rem 0;
    background-color: #000;
    color: #ccc;
    font-size: 0.8rem;

    .container {
        display: flex;
        justify-content: space-between;
    }

    .footer-link {
        color: #FC427B
    }

    .footer-link-img {
        width: 5rem;
        height: auto;
    }
}
</style>