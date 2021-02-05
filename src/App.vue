<template>
<div id="app">
    <!-- Particle Background -->
    <vue-particles style="z-index:1" class="particle" color="#bc97ec" linesColor="#ceb6ec"></vue-particles>
    <!-- The chat box -->
    <div style="display: table-cell; vertical-align: middle; position: relative; top: -5%;">
        <div>
            <h1 style="margin-bottom: 0;">Sandy Chatbot</h1>
            <h3>Your Chatting Companion</h3>
            <br />
        </div>
        <div>
            <basic-vue-chat style="z-index: 2" :title="'CHAT HERE'" :new-message="message" @newOwnMessage="onNewOwnMessage" />
        </div>
    </div>

</div>
</template>

<script>
import BasicVueChat from 'basic-vue-chat'
export default {
    name: 'App',
    components: {
        BasicVueChat,
    },
    data: function() {
        return {
            message: null,
            messages: []
        }
    },
    methods: {
        onNewOwnMessage: function(x) {
            this.messages.push(x)
            var context = this.messages.join(' ')
            let tokens = context.split(' ')
            let lo = Math.max(0, tokens.length - 30)
            let hi = tokens.length
            context = tokens.slice(lo, hi).join(' ')

            this.axios.get('https://chat-engine.sandychatbot.com/reply?context=' + context)
                .then((res) => {
                    var text = res['data']['response']
                    var d = new Date();
                    var time = d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds()
                    this.message = {
                        author: '',
                        contents: text,
                        date: time
                    };

                    // add the model response to the array
                    this.messages.push(text)

                })
                .catch((err) => console.log(err));
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

.basic-vue-chat {
    margin: 0 10px 0 10px;
}

.basic-vue-chat .window {
    margin: 0 auto;

}

.center-vertical {
    display: table-cell;
    vertical-align: middle;
}

.basic-vue-chat #window__messages__container,
.basic-vue-chat .window__input__container {
    background-color: white;
    z-index: 1;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    display: table;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
}
</style>
