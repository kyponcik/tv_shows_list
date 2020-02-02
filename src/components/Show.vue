<template>
  <div class="container">
    <h3>{{ serial.title }}</h3>
    <img :src="serial.image">
    <span>{{ serial.rating.toFixed(1) }}</span>
    <button @click="showInfo(serial.id)">{{ serial.titleOriginal }}</button>
    <modal
      :info="info"
      :modal="modalShow"
      @closeDialog="modalShow = $event"
    />
  </div>
</template>

<script>
  import Modal from './ModalDialog.vue'

  export default {
    name: 'Show',
    props: ['serial'],
    components: {
      Modal
    },
    data() {
      return {
        info: [],
        modalShow: false
      }
    },
    methods: {
      showInfo(id) {
        this.modalShow = true

        fetch('https://api.myshows.me/v2/rpc/', {
          method: 'POST',
          body: JSON.stringify({
            'jsonrpc': '2.0',
            'method': 'shows.GetById',
            'params': {
              'showId': id,
              'withEpisodes': true
            },
            'id': 1
          })
        })
          .then(r => r.json())
          .then(r => this.info = r.result)
      }
    }
  }
</script>

<style scoped>
  .container {
    display: flex;
    width: 300px;
    min-height: 350px;
    flex-direction: column;
    border: 1px solid rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    justify-content: space-between;
    padding: 10px;
    margin: 20px;

  }

  button {
    /*width:120px;*/
    /*align-self:center;*/
    min-height: 40px;
    background-color: rgb(64, 199, 129);
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 3px;
    box-shadow: 0 -3px rgb(53, 167, 110) inset;
    font-size: 15px;

  }

  h3 {
    border-bottom: 2px solid rgba(0, 0, 0, 0.2);
  }

  button:hover {
    background-color: rgb(53, 167, 110);
  }

  span {
    font-weight: bold;
  }

  img {
    height: 180px;
  }
</style>
