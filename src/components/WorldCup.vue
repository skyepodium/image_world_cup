<template>
  <div id="container">
    <div id="title">
      히가시노 게이고 월드컵 {{subInfo}}
    </div>


    <div id="images">

      <img src="../assets/icon/vs.png" id="vs_image">

      <div @click="itemClick(0)" id="left">
        <img :src="left.url" id="left_image">
      </div>

      <div @click="itemClick(1), show = !show" id="right">
        <img :src="right.url" id="right_image">
      </div>

    </div>

  </div>
</template>

<script>

  var bookList = [
    {
      title: '플레티나 데이터',
      url: require('../assets/bookList/data.jpg'),
    },
    {
      title: '탐정 갈릴레오',
      url: require('../assets/bookList/detective.jpg'),
    },
    {
      title: '예지몽',
      url: require('../assets/bookList/dream.jpg'),
    },
    {
      title: '악의',
      url: require('../assets/bookList/evil.jpg'),
    },
    {
      title: '게임의 이름은 유괴',
      url: require('../assets/bookList/game.jpg'),
    },
    {
      title: '졸업',
      url: require('../assets/bookList/graduation.jpg'),
    },
    {
      title: '기린의 날개',
      url: require('../assets/bookList/kirin.jpg'),
    },
    {
      title: '유성의 인연',
      url: require('../assets/bookList/meteor.jpg'),
    },
    {
      title: '나미야 잡화점의 기적',
      url: require('../assets/bookList/miracle.jpg'),
    },
    {
      title: '신참자',
      url: require('../assets/bookList/new.jpg'),
    },
    {
      title: '백야행',
      url: require('../assets/bookList/night.jpg'),
    },
    {
      title: '명탐점의 규칙',
      url: require('../assets/bookList/rule.jpg'),
    },
    {
      title: '성녀의 구제',
      url: require('../assets/bookList/saint.jpg'),
    },
    {
      title: '한여름의 방정식',
      url: require('../assets/bookList/summer.jpg'),
    },
    {
      title: '용의자 X의 헌신',
      url: require('../assets/bookList/suspect.jpg'),
    },
    {
      title: '라플라스의 마녀',
      url: require('../assets/bookList/witch.jpg'),
    },
  ];

  export default {
    name: 'WorldCup',
    data () {
      return {
        randomList: [],
        pickedList: [],
        bookList: bookList,
        left: {},
        right: {},
        currentRound: 16,
        currentIndex: 0,
        subInfo:'',
        show: true,
      }
    },
    methods: {
      makeRandomList(){
        let size = bookList.length;

        //1. 중복 검사를 위한 check배열 초기화
        let check = [];
        for(let i=0; i<size; i++) check.push(false);

        //2. 중복 되지 않게 랜덤한 리스트를 생성한다.
        for(let i=0; i<size; i++){

          //1) 랜덤 넘버를 생성한다.
          let randomNumber = Math.floor(Math.random() * size);

          //2) 만약 중복되어 있으면 다시 실행한다.
          if(check[randomNumber]){
            i--;
          }
          //3) 중복되지 않았으면 체크를 하고 randomList에 넣어준다.
          else{
            check[randomNumber] = true;
            this.randomList.push(this.bookList[randomNumber]);
          }
        }

        this.left = this.randomList[0];
        this.right = this.randomList[1];
      },

      //새로운 책들을 왼쪽과 오른쪽에 갱신한다.
      setItems: function() {
        this.left = this.randomList[this.currentIndex];
        this.right = this.randomList[this.currentIndex+1];
      },

      //책이 클릭되었을때 수행
      itemClick: function(num) {
//        setInterval(function(){ this.show = true; }, 2000);
        if(num === 0 ) console.log(this.left.title);
        else console.log(this.right.title);

        //num === 0 -> left, num ===1 -> right,
        if(num === 0 ) console.log('leftclick');
        else console.log('rightclick');

        //0,1 에 따라 pickedList에 정보를 넣어준다.
        if(num === 0 ) this.randomList.push(this.left);
        else this.randomList.push(this.right);

        //현 라운드의 마지막 대결이면
        if(this.currentIndex === this.currentRound-2){
          this.currentIndex = 0;
          this.currentRound /= 2;
          this.setSubInfo();
          this.makeRandomList();

          //기존 bookList를 비운다.
          this.bookList.splice(0, this.bookList.length);

          //pickedList를 기반으로 bookList를 갱신한다.
          for(let i=0; i<this.pickedList.length; i++){
            bookList.push(this.pickedList[i]);
          }

          //pickedList를 비운다.
          this.pickedList.splice(0, this.pickedList.length);
        }

        //현 라운드의 마지막 대결이 아니라면
        else{
          //다음 대결을 위해 인덱스 2증가
          this.currentIndex += 2;
          this.setSubInfo();
          this.setItems();
        }
        console.log(this.currentIndex);
        console.log(this.currentRound);
        console.log(this.bookList.length);
      },
      setSubInfo: function() {
        let ret = ' ';
        if(this.currentRound === 2){
          ret = '결승전';
        }
        else{
          ret =
            this.currentRound +'강 '
            +(this.currentIndex/2+1) + '/'
            + this.currentRound/2;
        }

        this.subInfo = ret;
      },
    },

    //페이지가 생성되면 기존 bookList를 섞는다.
    created: function() {
      this.makeRandomList();
      this.setSubInfo();
    },

  }
</script>

<style scoped>
  *{
    margin:0;
    padding:0;
  }
  #container{
    width:100%;
    height:100%;
    position: absolute;
    left:0;
    top:0;
  }
  #title{
    width:100%;
    height:50px;
    font-size:40px;
    background-color:rgba(0, 0, 0, 0.5);
    color:white;
    text-align: center;
    position:absolute;
    top:0;
    z-index:1;
  }
  #images{
    width: 100%;
    height: 100%;
    position:absolute;
    top:50px;
    left:0;
    background-color:pink;
  }
  #left{
    width: 50%;
    height:100%;
    background-color:orange;
    float:left;
  }
  #right{
    width: 50%;
    height:100%;
    background-color:lime;
    float:left;
  }
  #left_image{
    position: absolute;
    right:50%;
  }
  #vs_image{
    position:absolute;
    z-index:1;
    width:150px;
    height:103px;
    top:50%;
    left:50%;

    margin-left:-75px;
    margin-top:-51px;
  }


</style>
