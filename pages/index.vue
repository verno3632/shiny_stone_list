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
        :items="cards"
        item-key="name"
        show-select
        class="elevation-1"
      >
        <template v-slot:header.data-table-select> </template>
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
    if (!localStorage.getItem("selectedCards")) {
      return;
    }

    try {
      const selectedCards = JSON.parse(localStorage.getItem("selectedCards"));
      this.selected = this.cards.filter((card) => {
        return selectedCards.includes(card.name);
      });
    } catch (e) {}
  },
  watch: {
    selected: function (newSelected, oldSelected) {
      const selectedCards = newSelected.map((card) => card.name);
      const parsed = JSON.stringify(selectedCards);
      localStorage.setItem("selectedCards", parsed);
    },
  },
  data() {
    return {
      singleSelect: false,
      selected: [],
      headers: [
        {
          text: "レアリティ",
          align: "start",
          value: "rarity",
        },
        { text: "カード名", value: "name" },
        { text: "アイドル", value: "idol" },
        { text: "ユニット", value: "unit" },
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
