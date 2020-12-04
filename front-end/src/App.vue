<template>
<div id="app">
  <div class="header">
    <router-link to="/">
      <div class="logo">
        <img class="logopic" src="/images/logo2.png">
      </div>
    </router-link>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <div class='admin'>
    <router-link to="/admin">Manage</router-link>
    </div>
    <div class="github">
    <a href="https://github.com/raywray/creativeproject4.git"><u>GitHub</u></a>
    </div>
    <div class='AboutMe'>
    <router-link to="/aboutme">About Me</router-link>
    </div>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  justify-content: center;
  border: 1px solid black;
  color: #1C454F;
}

.logopic {
  margin-top: 25px;
  margin-bottom: 25px;
  height: 100px;
  width: auto;
}

.content {
padding: 20px 100px;
min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  justify-content: center;
  background: #e3e3e3;
  font-size: 12px;
  display: flex;

}

.admin {
  width: 33.3%;
  padding: 20px;
  text-align: center;
}

.AboutMe {
  width: 33.3%;
  padding: 20px;
  text-align: center;
}

.github {
  width: 33.3%;
  padding: 20px;
  text-align: center;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

</style>
