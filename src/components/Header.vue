<script>
  import siteLogo from '../assets/logo.svg'

  export default {
    name: 'Header',
    data: function() {
      return {
        siteLogo: siteLogo,
        toggleMenu: false
      }
    }
  }
</script>

<template>
  <header>
    <div class="container">
      <div class="header-content">
        <a href="/" class="navbar-logo">
          <img :src="siteLogo" class="logo" alt="Billsby" />
          <span class="t-logo">Billsby Card Tokenizer (Vue)</span>
        </a>
        <button class="toggle-menu burger-close" :class="{ 'burger-open': toggleMenu }"  @click="toggleMenu = !toggleMenu">
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
        <div class="menu-holder" :class="{ 'menu-open' : toggleMenu }">
          <a href="https://github.com/Billsby/billsby-tokenizer-vuejs" class="btn-white">Download on Github</a>
        </div>
      </div>
    </div>
  </header>
</template>

