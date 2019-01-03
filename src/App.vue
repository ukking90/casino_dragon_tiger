<template>
  <div id="app">
    <div>
      카드덱수: <input v-model="deckCount" type="number"/>
      <button @click="gameStart">게임시작</button>
    </div>
    <div>
      <input v-model="removeCardItem" type="text"/>
      <button @click="removeCard">카드삭제</button>
    </div>
    <div>
      <button @click="startDragonTiger">카드뽑기</button>
    </div>
    <div>
      <p>{{curDragon}} ~ {{curTiger}}</p>
      <p>{{cardResult}}</p>
    </div>
    <div>
      <ul>
        <li v-for="c in removeCardList">{{c}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
  const symbol = [{
    id: 'S', name: '스페이드', ename: 'spade',
  }, {
    id: 'H', name: '하트', ename: 'heart',
  }, {
    id: 'D', name: '다이아', ename: 'diamond',
  }, {
    id: 'C', name: '클로버', ename: 'club',
  }];
  const cardDeck = ['S1', 'S2', 'S3', 'S4', 'S5', 'S6', 'S7', 'S8', 'S9', 'S10', 'S11', 'S12', 'S13', 'H1', 'H2', 'H3', 'H4', 'H5', 'H6', 'H7', 'H8', 'H9', 'H10', 'H11', 'H12', 'H13', 'D1', 'D2', 'D3', 'D4', 'D5', 'D6', 'D7', 'D8', 'D9', 'D10', 'D11', 'D12', 'D13', 'C1', 'C2', 'C3', 'C4', 'C5', 'C6', 'C7', 'C8', 'C9', 'C10', 'C11', 'C12', 'C13'];


  export default {
    name: 'app',
    components: {},
    data() {
      return {
        deckCount: 8,
        cards: [],
        curDragon: null,
        curTiger: null,
        histories: [],
        removeCardList: [],
        removeCardItem: null,
        cardResult: '카드를 뽑아주세요',
      };
    },
    methods: {
      startDragonTiger() {
        let dragonCard = this.cards[0];
        let tigerCard = this.cards[1];
        this.curDragon = dragonCard
        this.curTiger = tigerCard
        if (dragonCard === tigerCard) {
          this.cardResult = '완벽한일치';
        } else {
          dragonCard = dragonCard.substring(1, dragonCard.length);
          tigerCard = tigerCard.substring(1, tigerCard.length);
          if (dragonCard === tigerCard) {
            this.cardResult = 'TIE';
          } else if (dragonCard > tigerCard) {
            this.cardResult = 'DRAGON 용승리';
          } else {
            this.cardResult = 'TIGER 호승리';
          }
        }
        this.histories.push(this.cardResult)
        this.shuffle(this.cards);
      },
      removeCard() {
        if (this.removeCardItem) {
          const val = this.removeCardItem.toUpperCase();
          this.removeCardList.push(val)
          this.removeCardItem = null;
          const index = this.cards.indexOf(val);
          this.cards.splice(index, 1);
          this.cardResult = '카드를 뽑아주세요';
        }
      },
      gameStart() {
        this.cards = [];
        for (let i = 0; i < this.deckCount; i++) {
          this.cards.push(...cardDeck);
          if (i === this.deckCount - 1) {
            this.shuffle(this.cards);
          }
        }
      },
      shuffle(a) {
        for (let i = a.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [a[i], a[j]] = [a[j], a[i]];
        }
        return a;
      },
    },
    mounted() {

    },
  };

</script>

<style>
</style>
