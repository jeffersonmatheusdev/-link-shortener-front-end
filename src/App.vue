<template>
  <div class="main">

    <div class="title">
      <a>ENCURTADOR</a>
    </div>

    <div class="short" v-if="encurtar">
      <a>Cole a URL para ser encurtada</a>
      <div>
        <input v-on:change="changeURL" />
        <button v-on:click="registerShortLink">Encurtar</button>
      </div>
    </div>

    <div class="shorted" v-else>
      <div>
        <a>Sua URL encurtada</a>
        <span>Copie o link encurtado e utilize.</span>
        <div>
          <input v-bind:value="shortLink"/>
          <button v-on:click="copyURL">Copiar URL</button>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      encurtar: true,
      link: null,
      shortLink: null
    }
  },
  methods: {
    changeURL(e) {
      this.link = e.target.value;
    },
    registerShortLink() {
      if (this.link) {
        fetch(`http://localhost:5555/c/${this.link}`)
          .then(data => data.json())
          .then(data => {
            this.shortLink = data.url;
          });
          this.encurtar = false;
      } else {
        alert('O valor do campo URL não pode estar em branco.');
      };
    },
    copyURL() {
      navigator.clipboard.writeText(this.shortLink).then(() => {
        alert('Link copiado!')
      })
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  border: 0;
  font-family: "Prompt";
}

:root {
  --bg-color: #6C716F;
  --text-color: #858b89ac;
  --bg-button-on: #1F7EE0;
  --bg-button: #4C98E6;
}

.main {
  width: 100vw;
  height: 100vh;
  background-image: linear-gradient(var(--bg-color), var(--text-color));
  display: flex;
  flex-direction: column;
}

.title {
  background-color: white;
  display: flex;
  margin: 4% auto 0 auto;
  height: 10%;
  width: 20%;
  justify-content: center;
  align-items: center;
  border-radius: 10px 0px 10px 10px;
}

.title>a,
.short>a {
  font-weight: 900;
  font-size: 2rem;
}

.short>a {
  font-size: 1.3rem;
  text-decoration: underline;
  text-decoration-color: white;
}

.short, .shorted {
  margin: auto;
  text-align: center;
}

.short>div>input {
  width: 45vw;
  height: 5vh;
  border-radius: 5px 0px 0px 5px;
  text-align: center;
}

.short>div>button {
  width: 5vw;
  height: 5vh;
  border-radius: 0px 5px 5px 0px;
  background-color: var(--bg-button);
  color: white;
  letter-spacing: 0.5px;
}

.short>div>button:hover {
  background-color: var(--bg-button-on);
  cursor: pointer;
}

.shorted > div {
  display: flex;
  flex-direction: column;
  text-align: start;
}

.shorted>div>a {
  font-size: 2rem;
  font-weight: 100;
}

.shorted>div>div>input {
  border-radius: 5px 0px 0px 5px;
  padding: 3%;
}

.shorted>div>div>button {
  border-radius: 0px 5px 5px 0px;
  padding: 3%;
  background-color: var(--bg-button);
  color: white;
}

.shorted>div>div>button:hover {
  background-color: var(--bg-button-on);
  cursor: pointer;
}
</style>