<template>
  <div>
   <div class="flex">
      <span>
        <img src="../assets/img/jdawgtor-chibi.jpg" width="200" />
      </span>
      <span style="padding: 0 20px;">
        <h2>Hi, I'm JDawgtor. I am an artist, composer, YouTuber, and Minecraft creator. 
          <br /><br />Come join me in my adventures in gaming, music, art, and movies. I'd love to have you!</h2>
      </span>
    </div>
    <h3>Tools</h3>
    <h2>Custom Structure Generator</h2>
    <p>
    Our first tool, this simple tool lets you upload your own custom structures and creates the code to spawn them randomly in Minecraft. It is simple enough that anyone can use it without knowing anything about data pack code or conventions. 
    <br /><br />Check it out here and let us know what you think:
    <br /><br /><a class="btn" href="/structure">Custom Structure Generator</a>
    </p>
  </div>
</template>

<script>
export default {}
</script>

<style>
.flex{display: flex; flex-direction: row;}
.home-links a {
  margin-right: 1rem;
}
.layout{height: 100%;}
.btn{cursor: pointer; color: #fff; text-shadow: 2px 2px 4px #000; background-image: linear-gradient(to right, #fcb9b9, #ffc0ac, #fecaa1, #f3d59b, #e2e29e, #d1eaab, #c0f0bb, #b2f5ce, #b1f6e0, #b8f5ee, #c4f3f6, #d4f1f8); font-size: 1.3rem; border: none; border-radius: 5px; padding: 5px 10px; text-decoration: none;}
.btn:hover{color: #000; background-image: linear-gradient(to left, #fcb9b9, #ffc0ac, #fecaa1, #f3d59b, #e2e29e, #d1eaab, #c0f0bb, #b2f5ce, #b1f6e0, #b8f5ee, #c4f3f6, #d4f1f8);}
</style>