<template>
  <div id="app">
    
    <!-- Particle Background -->
    <vue-particles class="particle" color="#bc97ec"  linesColor="#ceb6ec"></vue-particles>

    <div style="position: relative; height: 100vh;">
    <div class="center-vertical">
      <div>
        <h1 style="margin-bottom: 0;">Sandy Chatbot</h1>
        <h3>Your Chatting Companion</h3>
        <br/>
      </div>
      <div><basic-vue-chat :title="'CHAT HERE'" :new-message="message" @newOwnMessage="onNewOwnMessage" /></div>
    </div>
    </div>

  </div>
</template>

<script>
import BasicVueChat from 'basic-vue-chat'

// import VueFontAwesome from '@fortawesome/vue-fontawesome'

export default {
  name: 'App',
  components: {
    BasicVueChat,
    // VueParticles
    // VueFontAwesome
  },
  data: function(){
    return {
      message: null
    }
  },
  methods: {
    onNewOwnMessage: function (x) {
      this.axios.get('https://chat-engine.sandychatbot.com/reply?context='+x)
      .then((res) => {
        var text = res['data']['response']
        var d = new Date();
        var time = d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds()
        this.message = {
          author: '',
          contents: text,
          date: time
        };
      })
      .catch((err)=>console.log(err));
    }
  }
}
</script>

<style>

.particle {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
}

.over {
   position:absolute;
  top:0;
  left:0;
  right:0;
  bottom:0; 
}

.basic-vue-chat .window {
  margin: 0 auto;
  width: 700px;
  height: 500px;
  /* max-width: 350px; */
  /* height: 100%; */
}

.center-vertical {
  position: relative;
  top: 50%;
  -webkit-transform: translateY(-60%);
  -ms-transform: translateY(-60%);
  transform: translateY(-60%);
}

.basic-vue-chat #window__messages__container {
  background-color: white;
  z-index: 1;
}

.basic-vue-chat .window__input__container {
  background: white;
  z-index: 1;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
  /* position: relative; */
}

</style>
