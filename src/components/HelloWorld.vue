<!-- nécéssite l'importation de jQuery et Axios -->
<template>
  <button @click="UpdatePost" id="updateBtn">Actualiser</button>
  <button @click="Sort(this.reponse), Selected('A-to-Z-btn')" id="A-to-Z-btn">
    A &rarr; Z
  </button>
  <button
    @click="SortRev(this.reponse), Selected('Z-to-A-btn')"
    id="Z-to-A-btn"
  >
    Z &rarr; A
  </button>
  <button @click="SetModePro(), Selected('arrondirBtn')" id="arrondirBtn">
    Mode Pro
  </button>
  <input
    type="text"
    v-model="trieur"
    placeholder="Cherchez une devise ici"
    @change="this.Tri(this.trieur)"
  />
  <button @click="Tri(this.trieur)">Trier</button>
  <section id="crypto-main">
    <div class="items" v-for="crypto in reponse" v-bind:key="crypto.id">
      <div class="crypto_symbol">
        <span v-if="!modePro"
          >{{ crypto.symbol.slice(0, 6)
          }}<span v-if="crypto.symbol.length > 6">...</span></span
        >
        <span v-else>{{ crypto.symbol }}</span>
      </div>
      <div class="rest">
        <div class="lastPrice" @click="SetModePro">
          <span v-if="!modePro"
            >{{ parseFloat(crypto.lastPrice).toFixed(4) }} $</span
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
        <div class="crypto_avgPrice" @click="SetModePro">
          <span>Avg : </span>
          <span v-if="!modePro"
            >{{ parseFloat(crypto.weightedAvgPrice).toFixed(4) }}
            <span v-if="crypto.symbol.length <= 6">{{
              crypto.symbol.slice(-3)
            }}</span>
            <span v-else>{{ crypto.symbol.slice(-4) }}</span>
          </span>
          <span v-else>
            <span v-if="crypto.symbol.length <= 6"
              >{{ crypto.weightedAvgPrice }} {{ crypto.symbol.slice(-3) }}</span
            >
            <span v-else
              >{{ crypto.weightedAvgPrice }} {{ crypto.symbol.slice(-4) }}</span
            >
          </span>
        </div>
        <div class="modePro-infos" v-if="modePro">
          <div class="crypto_priceChange">
            Price change : {{ crypto.priceChange }}
          </div>
          <div class="crypto_volume">Volume : {{ crypto.volume }}</div>
          <div></div>
        </div>
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
        // next line allow to dev (see beforeMount()) 
        /* {
          symbol: "ETHBTC", //used
          priceChange: "0.00121100",
          priceChangePercent: "-1.560", // variation en % des dernières 24h
          weightedAvgPrice: "0.07852405", // prix moyen
          prevClosePrice: "0.07761100",
          lastPrice: "0.07882200", // dernier prix //used
          lastQty: "0.26640000", // dernière quantité
          bidPrice: "0.07882400", // prix de l'offre
          bidQty: "6.95970000", // quantité d'enchère de l'offre
          askPrice: "0.07882500", // prix de la demande
          askQty: "6.65090000", // quantité de la demande
          openPrice: "0.07761100", // prix d'ouverture
          highPrice: "0.07942700", // prix le plus bas
          lowPrice: "0.07702500", // prix le plus haut
          volume: "88228.93570000",
          quoteVolume: "6928.09329779",
          openTime: 1660224864370,
          closeTime: 1660311264370,
          firstId: 363558291,
          lastId: 363789064,
          count: 230774,
        },
        {
          symbol: "LTCBTC",
          priceChange: "0.00001300",
          priceChangePercent: "0.509",
          weightedAvgPrice: "0.00257442",
          prevClosePrice: "0.00255500",
          lastPrice: "0.00256800",
          lastQty: "8.23600000",
          bidPrice: "0.00256600",
          bidQty: "18.84000000",
          askPrice: "0.00256700",
          askQty: "34.77700000",
          openPrice: "0.00255500",
          highPrice: "0.00259500",
          lowPrice: "0.00253900",
          volume: "55189.66700000",
          quoteVolume: "142.08134077",
          openTime: 1660224863958,
          closeTime: 1660311263958,
          firstId: 82565804,
          lastId: 82582323,
          count: 16520,
        },*/
      ], 
      trieur: "",
      reponse: [],
      askedForData: false,
      btnList: [
        {
          label: "A-to-Z-btn",
          value: false,
        },
        {
          label: "Z-to-A-btn",
          value: false,
        },
        {
          label: "arrondirBtn",
          value: false,
        },
      ],
      modePro: false,
    };
  },
  methods: {
    UpdatePost() {
      // make a new request to the api
      if (!this.askedForData) {
        // launch the loading animation
        this.askedForData = true;
        setTimeout(() => {
          // and destroy it
          this.UpdatePost(), $("#loading").remove();
        }, 2500);
      }
      // getting data from the api
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
      // sort a to z
      list.sort((a, b) => {
        return a.symbol > b.symbol ? 1 : -1;
      });
    },
    SortRev(list) {
      // sort z to a
      list.sort((b, a) => {
        return a.symbol > b.symbol ? 1 : -1;
      });
    },
    Tri(trieur) {
      // search bar
      // symbols are in capital so we convert input to capital
      trieur = trieur.toUpperCase();
      // creating a table that will contain the result of search -> the goal is to keep data safe in the original table and do not make a new request to the APIs
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
    SetModePro(buttonId) {
      // switch between mode Pro and standard mode
      if (this.modePro) {
        $(".items").removeClass("items-modePro"), (this.modePro = false);
      } else {
        $(".items").addClass("items-modePro"), (this.modePro = true);
      }
    },
    Selected(buttonId) {
    // style stay for selected buttons
      let x = this.btnList.indexOf(buttonId);
      if (!this.btnList[x]) {
        $(buttonId).css("box-shadow", "none");
        for (var i = 0; i < this.btnList.length; i++) {
          if (this.btnList[i].label == buttonId) {
            // the variable is defined
            let rep = this.btnList[i].value;
            if (rep) {
              (this.btnList[i].value = false),
                $("#" + buttonId).css("box-shadow", "none");
            } else {
              (this.btnList[i].value = true),
                $("#" + buttonId).css(
                  "box-shadow",
                  "rgba(50, 50, 93, 0.25) 0px 0px 0px -102px inset, rgba(0, 0, 0, 0.3) 0px 8px 26px -8px inset"
                );
            }
          }
        }
        // if "a to z" selected -> unselect "z to a" and vice versa
        if (buttonId == this.btnList[1].label) {
          (this.btnList[0].value = false),
            $("#" + this.btnList[0].label).css("box-shadow", "none");
        } else if (buttonId == "A-to-Z-btn") {
          (this.btnList[1].value = false),
            $("#" + this.btnList[1].label).css("box-shadow", "none");
        }
      }
    },
  },
  beforeMount() {
    // next line allow to dev (see line 90)
    // this.reponse = this.cryptos;
  },
  mounted() {},
};

import { assertExpressionStatement } from "@babel/types";
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style src="@/assets/styles/style.css" scoped></style>
