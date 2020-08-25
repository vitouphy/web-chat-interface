<template>
  <div id="app">
    
    <!-- Particle Background -->
    <vue-particles class="particle" color="#bc97ec"  linesColor="#ceb6ec"></vue-particles>

    <div>
      <div>
        <h1 style="margin-bottom: 0;">Sandy Chatbot</h1>
        <h4 style="margin: 0;">Your Chatting Companion</h4>
        <br/>
      </div>
      <div><basic-vue-chat :title="'CHAT HERE'" :new-message="message" @newOwnMessage="onNewOwnMessage" /></div>
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

.row {
  /* display: flex; */
  display: inline-block;
}

.column {
  flex: 50%;
}

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
  max-width: 350px;
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
  margin-top: 60px;
}
</style>
