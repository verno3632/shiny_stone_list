<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="text-center">
        <logo />
        <vuetify-logo />
      </div>
      <v-data-table
        v-model="selected"
        :headers="headers"
        :items="ownedCards"
        item-key="name"
        show-select
        class="elevation-1"
      >
        <template v-slot:header.data-table-select> </template>
        <template v-slot:item.action="{ item }">
          <v-btn @click="changeToNotOwned(item)">未所持へ</v-btn>
        </template>
      </v-data-table>
      <v-data-table
        v-model="selected"
        :headers="headers"
        :items="notOwnedCards"
        item-key="name"
        class="elevation-1"
      >
        <template v-slot:item.action="{ item }">
          <v-btn @click="changeToOwned(item)">所持へ</v-btn>
        </template>
      </v-data-table>
    </v-col>
  </v-row>
</template>

<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";

export default {
  components: {
    Logo,
    VuetifyLogo,
  },
  mounted() {
    let ownedCards = [];
    if (localStorage.getItem("ownedCards")) {
      try {
        ownedCards = JSON.parse(localStorage.getItem("ownedCards"));
      } catch (e) {}
    }
    this.cardStatusList = this.cards.map((card) => {
      if (ownedCards.includes(card.name)) {
        card.owned = true;
      } else {
        card.owned = false;
      }

      return card;
    });

    let selectedCards = [];
    if (localStorage.getItem("selectedCards")) {
      try {
        selectedCards = JSON.parse(localStorage.getItem("selectedCards"));
      } catch (e) {}
    }
    this.selected = this.cards.filter((card) => {
      return selectedCards.includes(card.name);
    });
  },
  watch: {
    selected: function (newSelected, oldSelected) {
      const selectedCards = newSelected.map((card) => card.name);
      const parsed = JSON.stringify(selectedCards);
      localStorage.setItem("selectedCards", parsed);
    },
  },
  computed: {
    ownedCards: function () {
      return this.cardStatusList.filter((card) => {
        return card.owned;
      })
    },
    notOwnedCards: function () {
      return this.cardStatusList.filter((card) => {
        return !card.owned;
      })
    },
  },
  methods: {
    saveOwnedToLocal: function() {
      const ownedNameList = this.cardStatusList.filter((cs) => {
        return cs.owned
      }).map((card) =>{
        return card.name
      })
      const parsed = JSON.stringify(ownedNameList);
      localStorage.setItem("ownedCards", parsed);
    },
    changeToOwned: function (card) {
      this.cardStatusList = this.cardStatusList.map(cs => {
        if(cs.name == card.name){
          cs.owned = true
        }
        return cs
      })
      this.saveOwnedToLocal()
    },
    changeToNotOwned: function (card) {
      this.cardStatusList = this.cardStatusList.map(cs => {
        if(cs.name == card.name){
          cs.owned = false
        }
        return cs
      })
      this.saveOwnedToLocal()
    },
  },
  data() {
    return {
      singleSelect: false,
      selected: [],
      owned: [],
      cardStatusList: [],
      headers: [
        {
          text: "レアリティ",
          align: "start",
          value: "rarity",
        },
        { text: "カード名", value: "name" },
        { text: "アイドル", value: "idol" },
        { text: "ユニット", value: "unit" },
        { text: "", value: "action" },
      ],
      cards: [
        {
          rarity: "SSR",
          name: "ほわっとスマイル",
          idol: "櫻木真乃",
          unit: "イルミネーションスターズ",
        },
        {
          rarity: "SSR",
          name: "はじけてスマイル",
          idol: "櫻木真乃",
          unit: "イルミネーションスターズ",
        },
        {
          rarity: "SR",
          name: "ばってん長崎恋岬",
          idol: "月岡恋鐘",
          unit: "アンティーカ",
        },
      ],
    };
  },
};
</script>
