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
          name: "???",
          idol: "櫻木真乃",
          unit: "イルミネーションスターズ"
        },
        {
          rarity: "SSR",
          name: "ほわっとスマイル",
          idol: "櫻木真乃",
          unit: "イルミネーションスターズ"
        },
        {
          rarity: "SSR",
          name: "???",
          idol: "櫻木真乃",
          unit: "イルミネーションスターズ"
        },
        {
          rarity: "SSR",
          name: "はじけてスマイル",
          idol: "櫻木真乃",
          unit: "イルミネーションスターズ"
        },
        {
          rarity: "SR",
          name: "ばってん長崎恋岬",
          idol: "月岡恋鐘",
          unit: "アンティーカ"
        },
      ],
    };
  },
};
</script>
