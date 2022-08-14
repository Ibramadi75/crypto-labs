<!-- nécéssite l'importation de jQuery et Axios -->
<template>
  <button @click="UpdatePost" id="updateBtn">Actualiser</button>
  <button @click="Sort(this.reponse), Selected('A-to-Z-btn')" id="A-to-Z-btn">A &rarr; Z</button>
  <button @click="SortRev(this.reponse), Selected('Z-to-A-btn')" id="Z-to-A-btn">Z &rarr; A</button>
  <button @click="Arrondir(), Selected('arrondirBtn')" id="arrondirBtn">Mode Pro</button>
  <input
    type="text"
    v-model="trieur"
    placeholder="Cherchez une devise ici"
    @change="this.Tri(this.trieur)"
  />
  <button @click="Tri(this.trieur)">Trier</button>
  <section id="crypto-main">
    <div class="items" v-for="crypto in reponse" v-bind:key="crypto.id">
      <div class="symbols" >
        <span v-if="arrondir">{{ crypto.symbol.slice(0,6) }}<span v-if="crypto.symbol.length > 6">...</span></span>
        <span v-else>{{ crypto.symbol }}</span>
      </div>
      <div class="rest">
        <div class="lastPrice" @click="Arrondir">
          <span v-if="arrondir">{{ parseFloat(crypto.lastPrice).toFixed(4) }} $</span
          >
          <span v-else>{{ crypto.lastPrice }}$</span>
        </div>
        <div class="priceChangePercent">
          <span>{{ crypto.priceChangePercent }} % </span>
          <img
            v-if="crypto.priceChangePercent[0] != '-'"
            class="rowEvolve"
            src="../../public/images/augmenter.png"
            width="20"
          />
          <img
            v-if="crypto.priceChangePercent[0] == '-'"
            class="rowEvolve"
            src="../../public/images/augmenter.png"
            width="20"
            style="transform: scaleY(-1)"
          />
        </div>
        <div class="avgPrice" @click="Arrondir">
          <span>Avg : </span>
          <span v-if="arrondir"
            >{{ parseFloat(crypto.weightedAvgPrice).toFixed(4) }} $</span
          >
          <span v-else>{{ crypto.weightedAvgPrice }}$</span>
        </div>
        <div class="graphs"></div>
      </div>
    </div>
  </section>
  <div v-if="this.askedForData" id="loading"></div>
</template>
<script>
export default {
  data() {
    return {
      cryptos: [
        // {
        //   symbol: "ETHBTC",
        //   priceChange: "0.00121100",
        //   priceChangePercent: "-1.560", // variation en % des dernières 24h
        //   weightedAvgPrice: "0.07852405", // prix moyen
        //   prevClosePrice: "0.07761100",
        //   lastPrice: "0.07882200", // dernier prix
        //   lastQty: "0.26640000", // dernière quantité
        //   bidPrice: "0.07882400", // prix de l'offre
        //   bidQty: "6.95970000", // quantité d'enchère de l'offre
        //   askPrice: "0.07882500", // prix de la demande
        //   askQty: "6.65090000", // quantité de la demande
        //   openPrice: "0.07761100", // prix d'ouverture
        //   highPrice: "0.07942700", // prix le plus bas
        //   lowPrice: "0.07702500", // prix le plus haut
        //   volume: "88228.93570000",
        //   quoteVolume: "6928.09329779",
        //   openTime: 1660224864370,
        //   closeTime: 1660311264370,
        //   firstId: 363558291,
        //   lastId: 363789064,
        //   count: 230774,
        // },
        // {
        //   symbol: "LTCBTC",
        //   priceChange: "0.00001300",
        //   priceChangePercent: "0.509",
        //   weightedAvgPrice: "0.00257442",
        //   prevClosePrice: "0.00255500",
        //   lastPrice: "0.00256800",
        //   lastQty: "8.23600000",
        //   bidPrice: "0.00256600",
        //   bidQty: "18.84000000",
        //   askPrice: "0.00256700",
        //   askQty: "34.77700000",
        //   openPrice: "0.00255500",
        //   highPrice: "0.00259500",
        //   lowPrice: "0.00253900",
        //   volume: "55189.66700000",
        //   quoteVolume: "142.08134077",
        //   openTime: 1660224863958,
        //   closeTime: 1660311263958,
        //   firstId: 82565804,
        //   lastId: 82582323,
        //   count: 16520,
        // },
      ],
      trieur: "",
      reponse: [],
      arrondir: true,
      askedForData: false,
      btnList: [
        {
          label : 'A-to-Z-btn', value : false
        },
        {
          label : 'Z-to-A-btn', value : false
        },
        {
          label : 'arrondirBtn', value : false
        }
      ]
    };
  },
  methods: {
    UpdatePost() {
      if (!this.askedForData){
        this.askedForData = true;
        setTimeout(
          () => {this.UpdatePost(), $( '#loading' ).css('opacity', '0') }, 2500
          ); 
      }
      axios
        .get("https://api2.binance.com/api/v3/ticker/24hr")
        .then(
          (response) => (this.cryptos = response.data),
          (this.reponse = this.cryptos)
        )
        .catch(function (error) {
          // handle error
          console.log(error);
        })
        .then(function () {
          // always executed
        });

      // setTimeout(
        // () => {this.UpdatePost()}, 4000
      // ); 
    },
    Sort(list) {
      list.sort((a, b) => {
        return a.symbol > b.symbol ? 1 : -1;
      });
    },
    SortRev(list) {
      list.sort((b, a) => {
        return a.symbol > b.symbol ? 1 : -1;
      });
    },
    Tri(trieur) {
      trieur = trieur.toUpperCase();
      let reponse = [];
      for (var i = 1; i < this.cryptos.length; i++) {
        let x = this.cryptos[i];
        let y = x.symbol;
        if (y.includes(trieur)) {
          reponse.push(x);
        }
      }
      return (this.reponse = reponse);
    },
    Arrondir(buttonId) {
      if (this.arrondir){
        this.arrondir = false, $( '.items' ).css('width', '25%');
      } else{
        this.arrondir = true,
        $( '.items' ).css('width','20%')
      }
    },
    Selected(buttonId) {
      let x = this.btnList.indexOf(buttonId);
      if (this.btnList[x] == false){
        
      }else{
         $( buttonId ).css('box-shadow', 'none');
          for (var i = 0; i < this.btnList.length; i++){
            if (this.btnList[i].label == buttonId) {
              // the variable is defined
              let rep = this.btnList[i].value;
              if (rep){
                this.btnList[i].value = false,
                $( '#' + buttonId ).css('box-shadow', 'none')
              }else{
                this.btnList[i].value = true, 
                $( '#' + buttonId ).css('box-shadow', 'rgba(50, 50, 93, 0.25) 0px 0px 0px -102px inset, rgba(0, 0, 0, 0.3) 0px 8px 26px -8px inset');
              }
            }
          }
          if(buttonId == this.btnList[1].label){
              this.btnList[0].value = false,
              $( '#' + this.btnList[0].label ).css('box-shadow', 'none')
            }else if(buttonId == 'A-to-Z-btn'){
              this.btnList[1].value = false,
              $( '#' + this.btnList[1].label ).css('box-shadow', 'none')
            }
        }
      }
    },
  beforeMount() {
  },
  mounted() {},
};

import { assertExpressionStatement } from "@babel/types";
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#crypto-main {
  width: 80%;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: block;
  margin: 20px 10px;
}
a {
  color: #42b983;
}
.items {
  width: 20%;
  height: 70px;
  border-radius: 15px;
  background: #f5f5f5e0;
  min-width: 200px;
  margin: 1%;
  padding-left: 20px;
  font-size: 12px;
  margin-top: 50px;
}
.items > div {
  cursor: pointer;
  display: flex;
  align-items: center;
  margin-right: 5px;
}
#crypto-main {
  margin: auto;
  margin-top: 50px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
}
.symbols {
  color: rgba(0, 0, 0, 0.795);
  font-size: 18px;
  font-weight: 700;
}
.items {
  display: flex;
  align-items: center;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 13px 9px 0px;
}
.rest {
  margin: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
img {
  vertical-align: bottom;
}
.avgPrice {
  margin-top: 5px;
    white-space: nowrap;
}
#loading{
  width: 100px;
  height: 10px;

  position: fixed;
  top: 50%;
  left: 45%;
  transform: translate(-50%, -50%);

  border-radius: 10px;
}
#loading::before{
  background-color:rgba(53, 66, 187, 0.39);

  position: absolute;
  content: " ";
  width: 10px;
  height: 10px;

  animation: loadingAnimation infinite 1s linear;
}
@keyframes loadingAnimation {
  0%{
    width: 10px;
    margin-left: 90px;
  }
  25%{
    width:100px;
    margin-left: 0%;
  }
  50%{
    width:10px;
    margin-left: 0%;
  }
  75%{
    width:100px;
    margin-left: 0%;
  }
  100%{
    width:10px;
    margin-left: 90px;
  }
}
</style>