<template>
<div>
  <div>
    <div
        class="sortable-ul"
    >
      <div
          class="item__cart"
          @dragstart="(e) => dragstartHandler(e, card)"
          @dragleave="(e) => dragleaveHandler(e)"
          @dragend="(e) => dragendHandler(e)"
          @dragover="(e) => dragoverHandler(e, card)"
          @drop="(e) => dropHandler(e, card)"
          v-for="card in cards.sort(sortCards)"
          :key="card"
          draggable=true
      >
        <p>
          <i class="fi fi-rr-menu-burger"></i>
          {{card.city}}
        </p>
        <i @click="removeCart()" class="fi fi-bs-trash"></i>
      </div>
    </div>
  </div>
  <div class="add">
    <p>Add Location</p>
    <div class="add_inp">
      <input placeholder="City.." v-model="newCity"/>
      <i @click="addCard" class="fi fi-rr-undo-alt"></i>
    </div>
  </div>
</div>
</template>

<script>


export default {
  name: "createPost",
  data(){
    return{
      newCity: '',
      newCard:{id: 0, order: 0, city: ''},
      actId: 3,
      actOrder: 0,
      currentCards: []
    }
  },
  props:{
    cards:{
      type: Array
    }
  }
,
  methods:{
    dragstartHandler(e, card){
      console.log('drag', card)
      this.currentCards = card
    },
    dragleaveHandler(e){},
    dragendHandler(e){
      e.target.style.background = '#fff'
    },
    dragoverHandler(e){
      e.preventDefault()
      e.target.style.background = '#eee'
    },
    dropHandler(e, card){
      e.preventDefault()
      this.cards = this.cards.map(c => {
        if (c.id === card.id){
          return {...c, order: card.order}
        }
        if (c.id === this.currentCards.id){
          return {...c, order: card.order}
        }
        return c
      })
      e.target.style.background = '#fff'
      console.log('drop', card)
      console.log(this.cards)
    },
    sortCards(a, b){
      if (a.order > b.order){
        return 1
      }else {
        return -1
      }
    },
    addCard(){
      this.actId++
      this.actOrder++
      this.newCard.id = this.actId
      this.newCard.order = this.actOrder
      this.newCard.city = this.newCity
      this.cards.push(this.newCard)
      this.newCity = ''
      this.newCard = {id: 0, order: 0, city: ''}
    }
  },
}
</script>

<style scoped>
@import url('https://cdn-uicons.flaticon.com/uicons-regular-rounded/css/uicons-regular-rounded.css');
@import url('https://cdn-uicons.flaticon.com/uicons-regular-rounded/css/uicons-regular-rounded.css');
@import url('https://cdn-uicons.flaticon.com/uicons-bold-straight/css/uicons-bold-straight.css');

p{
  margin: 0;
}
.item__cart{
  text-decoration: none;
  display: flex;
  justify-content: space-between;
  margin: 8px 0;
  padding: 8px 10px;
  background-color: #efefef;
}
.item__cart i{
  position: relative;
  top: 3px;
  color: #333;
  margin: 0 5px;
}
.item__cart i:hover{
  color: #0ad2d2;
  cursor: pointer;
}
.item__cart:hover{
  cursor: pointer;
}
.add{
  margin: 15px 0;
}
.add p{
  color: black;
  font-weight: 600;
}
.add_inp{
  margin: 8px 0;
  display: flex;
}
.add_inp i{
  margin: 0 10px;
}
.add_inp i:hover{
  color: #0ad2d2;
  cursor: pointer;
}
.add_inp input{
  padding: 5px;
  border-radius: 3px;
  border: 1px #eee solid;
  width: 100%;
}
.add_inp input:active{
  border: #0ad2d2 solid 1px;
}
</style>