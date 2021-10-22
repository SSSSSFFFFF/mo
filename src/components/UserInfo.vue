<script setup>
import { ref, onMounted, onUpdated, onUnmounted } from 'vue'
const w = ref(5000);
const earn = ref(0);
const day = ref(0);
const showPop = ref(false);
let interval = null;
onMounted(() => {
    console.log('mounted!')
    time()
})
onUpdated(() => {
    // console.log('updated!')
})
onUnmounted(() => {
    end()
})

const cards = [
    {
        name: '1060',
        prize: 1700,
        earn: 80,
        sell: 1500,
        num:0,
        // 0.047
    },
    {
        name: '1070',
        prize: 2400,
        earn: 120,
        sell: 2000,
        num:0,
        // 0.045
    },
    {
        name: '1080',
        prize: 4000,
        earn: 150,
        sell: 3000,
        num:0,
        // 0.0375
    },
    {
        name: '2060',
        prize: 3000,
        earn: 120,
        sell: 2600,
        num:0,
        // 0.04
    },
    {
        name: '2070',
        prize: 4000,
        earn: 130,
        sell: 3000,
        num:0,
        // 0.0325
    },
    {
        name: '2080',
        prize: 6000,
        earn: 200,
        sell: 5000,
        num:0,
        // 0.033
    },
        {
        name: '3060',
        prize: 4500,
        earn: 190,
        sell: 3600,
        num:0,
        //0.042
    },
    {
        name: '3070',
        prize: 6500,
        earn: 250,
        sell: 5000,
        num:0,
        // 0.038
    },
    {
        name: '3080',
        prize: 8000,
        earn: 300,
        sell: 7000,
        num:0,
        // 0.0375
    },
]

function end(){
    clearInterval(interval)
    interval = null;
    showPop.value = true
}

function time(){
    interval = setInterval(() => {
        if (day.value == 100){
            end()
        } else {
            w.value += earn.value
            day.value ++
        }
    }, 1000);
}

function buy(card,index){
    if(w.value - card.prize > 0){
        cards[index].num ++;
        w.value -= card.prize
        earn.value += card.earn
    }
}

function sell(card,index){
    if(card.num > 0){
        cards[index].num --;
        w.value += card.sell
        earn.value -= card.earn
    }
}

function remake(){
    window.location.reload()
}
</script>

<template>
  <div>day{{day}} money{{w}}</div>
  <div>+{{earn}}/day</div>
  <br>
  <div  class="cards">
    <div v-for="(item,index) in cards" class="card">
        <div>{{item.name}} x {{item.num}}</div>
        <div> +{{item.earn}}/day</div>
        <button @click="buy(item,index)">buy -{{item.prize}}</button>
        <button @click="sell(item,index)">sell +{{item.sell}}</button>
    </div>
  </div>
  <div class="popUp" v-show="showPop">
    <div>score: {{w}}</div>
    <button @click="remake">remake</button>
  </div>
</template>

<style scoped>
.cards {
    display: flex;
    flex-wrap: nowrap;
    padding: 100px;
}

.card {
    margin: 30px;
}

.popUp {
    border:1px solid black;
    background-color: white;
    padding: 50px;
    position: absolute;
    left: 50%;
    top: 40%;
    transform: translate(-50%,-50%);
}
</style>
