<template>
  <div @click="closeDialog" class="modal" v-if="modal">
    <div @click.stop class="modal-content">
      <div @click="closeDialog" class="close">{{ close }}</div>
      <h3 class="title">{{ info.title }}</h3><br>
      <span class="status">
				{{ getTranslate(info.status) }}
			</span>
      <img :src="info.image">
      <p class="discription" v-html="info.description"></p>
      <h3>Список эпизодов</h3>
      <ul class="episode-list">
        <li class="episode" v-for="episode in info.episodes">
          <span class="short-name">{{ episode.shortName }}</span>
          <span class="episode-title">{{ episode.title }}</span>
          <span class="episode-status">{{ episodeStatus(episode.airDate) }}</span>
        </li>
      </ul>
      <button @click="closeDialog" class="close-btn">{{ close }}</button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Modal',
    props: ['modal', 'info'],
    data(){
      return{
        close:'CLOSE'
      }
    },
    methods: {
      closeDialog() {
        this.$emit('closeDialog', false)
      },
      getTranslate(str) {
        switch (str) {
          case 'In Development':
            return 'В зазработке'
          case 'Canceled/Ended':
            return 'Закончен/Отменен'
          case 'Returning Series':
            return 'Сериал продолжается'
          default:
            return str
        }
      },
      episodeStatus(date){
          const showsDate = new Date(date)
          if(Date.now() > showsDate){
            return 'Серия вышла'
          }else{
           return  showsDate.getDate() + '.' + (showsDate.getMonth() + 1) + '.' + showsDate.getFullYear()
          }    
      }
    }
  }
</script>

<style scoped>
  .modal {
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    position: fixed;
    overflow: auto;
    z-index: 1;
    top: 0;
    left: 0;
  }
  .modal-content {
    margin: 15% auto;
    background-color: #fefefe;
    padding: 20px;
    border: 1px solid rgba(0, 0, 0, 0.5);
    width: 70%;
    display: flex;
    flex-direction: column;
  }
  .close {
    align-self: flex-end;
    font-size: 10px;
    color: #aaa;
    font-weight: bold;
    cursor: pointer;
  }
  .episode-list {
    list-style: none;
    align-self: center;
    display: block;
    width: 70%;
  }
  .modal-content img {
    width: 80%;
    align-self: center;
    margin-top:10px;
  }
  .discription{
    width:80%;
    align-self: center;
  }
  .close-btn {
    width:100px;
    align-self: center;
    background-color: #9147ff;
    color:#fefefe;
    border:none;
    height: 30px;
  }
  .close-btn:hover{
    background-color: #772ce8;
  }
  .episode {
    align-self: flex-start;
    text-align: left;
    display: block;
    border: 2px solid #9147ff;
    border-radius: 10px;
    margin-bottom: 15px;
    padding:10px;
    position:relative;

  }
  .episode:nth-child(odd){
    background-color:rgba(0, 0, 0, 0.1);
  }
  .episode .short-name {
    font-weight: bold;
    font-size:12px;
    position:absolute;
    top:-8px;
    z-index: 1;
    background-color:#fefefe;
    padding-left:2px;
    padding-right:2px;
    color:black;
  }
  .episode .episode-status{
    float:right;
    font-weight:bold; 
  }
  .episode-title{
    color:black;
    font-weight: bold;
  }

</style>
