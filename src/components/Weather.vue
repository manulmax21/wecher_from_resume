<template>
  <my-dialog v-model:show="hiddenDialog" >
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
              v-for="card in items.sort(sortCards)"
              :key="card"
              draggable=true
          >
            <p>
              <i class="fi fi-rr-menu-burger"></i>
              {{card.city}}
            </p>
            <i @click="removeCart(card)" class="fi fi-bs-trash"></i>
          </div>
        </div>
      </div>
      <div class="add">
        <p>Add Location</p>
        <div class="add_inp">
          <input type="text" placeholder="City.." v-model="newCity"/>
          <i @click="addCard" class="fi fi-rr-undo-alt"></i>
        </div>
      </div>
    </div>
  </my-dialog>
  <div class="container">
    <div class="setting">
      <i @click="hiddenDialog = true" class="fi fi-rr-settings"></i>
    </div>
    <div >
      <weather-component
          :src="src"
          v-for="item in items"
          :key="item"
          :city="item.city"
      />
    </div>
  </div>
</template>

<script>
import WeatherComponent from "@/components/WeatherComponent";
import MyDialog from "@/components/MyDialog";
import createPost from "@/components/createPost";

export default {
  name: "Weather",
  props: {
    src:{
      type: String
    }
  },
  data(){
    return{
      items: [
        {id: 0, order: 1, city: 'Stavropol'},
        {id: 1, order: 2, city: 'London'},
        {id: 3, order: 4, city: 'Moscow'},
      ],
      newCity: '',
      newCard:{id: 0, order: 0, city: ''},
      actId: 3,
      actOrder: 0,
      currentCards: [],
      hiddenDialog: false
    }
  },
  components: {WeatherComponent, MyDialog, createPost},
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
      this.items = this.items.map(c => {
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
      console.log(this.items)
      this.saveCart()
    },
    sortCards(a, b){
      if (a.order > b.order){
        return 1
      }else {
        return -1
      }
    },
    addCard(){
      if (!this.newCity) return
      this.actId++
      this.actOrder++
      this.newCard.id = this.actId
      this.newCard.order = this.actOrder
      this.newCard.city = this.newCity
      this.items.push(this.newCard)
      this.newCity = ''
      this.newCard = {id: 0, order: 0, city: ''}
      this.saveCart()
    },
    removeCart(card){
      this.items = this.items.filter(p => p.id !== card.id)
      this.saveCart()
    },
    saveCart(){
      let parsed = JSON.stringify(this.items)
      localStorage.setItem('items', parsed)
    }
  },
  mounted() {
    if (localStorage.getItem('items')){
      try {
        this.items = JSON.parse(localStorage.getItem('items'))
      }catch (e){
        localStorage.getItem('items')
      }
    }
  }

}
</script>

<style scoped>
@import url('https://cdn-uicons.flaticon.com/uicons-regular-rounded/css/uicons-regular-rounded.css');

.container{
  width: 100%;
  max-width: 240px;
  margin: 27px auto;
  padding: 0 34px;
}
.setting{
  text-align: right;
  font-size: 20px;
}
.setting i:hover{
  color: #0ad2d2;
  cursor: pointer;
}
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