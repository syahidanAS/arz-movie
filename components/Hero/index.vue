<template>
  <div class="hero">
    <iframe
      src="https://www.youtube.com/embed/CaimKeDcudo?autoplay=1&mute=1&controls=0&showinfo=0&loop=1&modestbranding=1&autohide=1&playlist=CaimKeDcudo"
      title="YouTube video player"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    ></iframe>
    <div class="text-container">
      <div class="text">
        <h1><span>Now </span>Showing</h1>
        <a href="#movie-grid" class="button">View Movies</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeroComp',
}
</script>

<style lang="scss" scoped>
@import './hero.scss';
</style>