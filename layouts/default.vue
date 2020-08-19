<template>
  <div class="layout">
    <header class="header">
      <a class="site-link" href="/"><strong>JDawgtor</strong></a>
      <div><a class="nav__link" href="mailto:art@jdawgtor.com">Email Us</a></div>
      <nav class="nav">
        <a class="nav__link" href="/structure/">Structure Gen</a>
      </nav>
    </header>
     <Nuxt />
  </div>
</template>

<style>
body {
  font-family: -apple-system,system-ui,BlinkMacSystemFont,"Segoe UI",Roboto,"Helvetica Neue",Arial,sans-serif;
  margin:0;
  padding:0;
  line-height: 1.5;
  color: #FFFFFF;
  text-shadow: 2px 2px 4px #000;
  background-image: url('../assets/img/sgen-bg.jpg');
  background-size: 100% 100%;
  background-attachment: fixed;
  display:flex !important; 
  flex-direction: row;
  justify-content: center;
  min-height: 100vh !important; 
}
h1{font-size: 1.7rem;}
h2{font-size: 1.5rem;}
h3{font-size: 1.3rem;}

.layout {
  width: 1000px;
  min-height: 1170px;
  margin: 0 auto;
  padding-left: 20px;
  padding-right: 20px;
  background-image: linear-gradient(to left top, #154eb7, #285bc1, #3768cc, #4476d6, #5283e0, #5e8de5, #6996ea, #75a0ef, #81a8f0, #8db0f2, #98b8f3, #a4c0f4);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  height: 30px;
}

.nav__link {
  margin-left: 20px;
  color: #FFF;
  text-decoration: underline;
}

.site-link{color: #FFF; text-decoration: none; font-size: 1.4rem;}
::-webkit-input-placeholder{color: #000; font-style: italic}
:-ms-input-placeholder {color: #000; font-style: italic}
::placeholder {color: #000; font-style: italic}
.link, .link:hover,.link:visited{color: #FFF;}
</style>
