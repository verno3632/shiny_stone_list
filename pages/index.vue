<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="10">
      <v-data-table
        v-model="selected"
        :headers="headers"
        :items="ownedCards"
        :hide-default-footer="true"
        item-key="name"
        show-select
        disable-pagination
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
        :hide-default-footer="true"
        item-key="name"
        disable-pagination
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
export default {
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
        { rarity: "SSR", name: "【ほわっとスマイル】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【ハ♡トフェルトゥギフト】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【はじけてスマイル】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【深染め、いろみぐさ】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【ソラを跳ね】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【柔らかな微笑み】風野灯織", idol: "風野灯織", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【淡雪の戯れ】風野灯織", idol: "風野灯織", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【伸ばす手に乗せるのは】風野灯織", idol: "風野灯織", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【清閑に息をひそめて】風野灯織", idol: "風野灯織", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【金色の元気いっぱいガール】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【夏に恋するピチカート！】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【シュカのまにまに】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【日々を紡ぐインヴェルノ】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【アイの誓い】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SSR", name: "【ばりうまかブルース】月岡恋鐘", idol: "月岡恋鐘", unit: "アンティーカ", },
        { rarity: "SSR", name: "【月の浜辺で待っとって】月岡恋鐘", idol: "月岡恋鐘", unit: "アンティーカ", },
        { rarity: "SSR", name: "【風吹く丘にはよ来んね】月岡恋鐘", idol: "月岡恋鐘", unit: "アンティーカ", },
        { rarity: "SSR", name: "【うちのリボンは恋結び】月岡恋鐘", idol: "月岡恋鐘", unit: "アンティーカ", },
        { rarity: "SSR", name: "【トリッキーナイト】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SSR", name: "【摩的・アンチテーゼ】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SSR", name: "【パープル・ミラージュ】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SSR", name: "【フィドル・ファドル】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SSR", name: "【真・TRAVELER】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SSR", name: "【紺碧のボーダーライン】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SSR", name: "【1/60・NaturalHeart】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SSR", name: "【幸福のリズム】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SSR", name: "【海と太陽のプロメッサ】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SSR", name: "【乙女と交わすTrick】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SSR", name: "【お試し/みつゴコロ】三峰結華", idol: "三峰結華", unit: "アンティーカ", },
        { rarity: "SSR", name: "【それなら目をつぶりましょう】三峰結華", idol: "三峰結華", unit: "アンティーカ", },
        { rarity: "SSR", name: "【NOT≠EQUAL】三峰結華", idol: "三峰結華", unit: "アンティーカ", },
        { rarity: "SSR", name: "【く ら く ら】三峰結華", idol: "三峰結華", unit: "アンティーカ", },
        { rarity: "SSR", name: "【雨に祝福】三峰結華", idol: "三峰結華", unit: "アンティーカ", },
        { rarity: "SSR", name: "【霧・音・燦・燦】幽谷霧子", idol: "幽谷霧子", unit: "アンティーカ", },
        { rarity: "SSR", name: "【夕・音・鳴・鳴】幽谷霧子", idol: "幽谷霧子", unit: "アンティーカ", },
        { rarity: "SSR", name: "【菜・菜・輪・舞】幽谷霧子", idol: "幽谷霧子", unit: "アンティーカ", },
        { rarity: "SSR", name: "【鱗・鱗・謹・賀】幽谷霧子", idol: "幽谷霧子", unit: "アンティーカ", },
        { rarity: "SSR", name: "【第2形態アーマードタイプ】小宮果穂", idol: "小宮果穂", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【新装備・バブルバスター！】小宮果穂", idol: "小宮果穂", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【潮騒のシーショア】小宮果穂", idol: "小宮果穂", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【エクストリーム・ブレイク！】小宮果穂", idol: "小宮果穂", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【ちょこ色×きらきらロマン】園田智代子", idol: "園田智代子", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【あそ→と♡ちよこれいと】園田智代子", idol: "園田智代子", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【かきまぜたら＊ミルク】園田智代子", idol: "園田智代子", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【You're My Dream】園田智代子", idol: "園田智代子", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【ラムネ色の覚悟】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【とびっきりジンジャー】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【曲がり角のランウェイ】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【誰が為シンパシー】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【秋陽のスケッチ】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【杜野凛世の印象派】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【凛世花伝】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【水色感情】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【十二月短篇】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【われにかへれ】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【アルティメットマーメイド】有栖川夏葉", idol: "有栖川夏葉", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【メイドインナツハ】有栖川夏葉", idol: "有栖川夏葉", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【Ring a bell】有栖川夏葉", idol: "有栖川夏葉", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【♡AKQJ10】有栖川夏葉", idol: "有栖川夏葉", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【cheer＋】有栖川夏葉", idol: "有栖川夏葉", unit: "放課後クライマックスガールズ", },
        { rarity: "SSR", name: "【スタンバイオッケー】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【ゆらゆらアクアリウム】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【ないしょのスイーツ】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【お散歩サンライト】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【叶えて☆ゴールドフィッシュ】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【事務所。静寂。大輪の華】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【好きなものはなんですか？】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【I♡DOLL】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【四夜一夜物語】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【マイ・ピュア・ロマンス】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【シークレット・ヒロイン】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【サマーハニー・シーズン】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【はるかぜまち、1番地】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【秋空と紅葉】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SSR", name: "【ジャンプ！スタッグ！！！】芹沢あさひ", idol: "芹沢あさひ", unit: "ストレイライト", },
        { rarity: "SSR", name: "【空と青とアイツ】芹沢あさひ", idol: "芹沢あさひ", unit: "ストレイライト", },
        { rarity: "SSR", name: "【不機嫌なテーマパーク】芹沢あさひ", idol: "芹沢あさひ", unit: "ストレイライト", },
        { rarity: "SSR", name: "【オ♡フ♡レ♡コ】黛冬優子", idol: "黛冬優子", unit: "ストレイライト", },
        { rarity: "SSR", name: "【アンシーン・ダブルキャスト】黛冬優子", idol: "黛冬優子", unit: "ストレイライト", },
        { rarity: "SSR", name: "【ちょっとあげる〜】和泉愛依", idol: "和泉愛依", unit: "ストレイライト", },
        { rarity: "SSR", name: "【メイ・ビー】和泉愛依", idol: "和泉愛依", unit: "ストレイライト", },
        { rarity: "SSR", name: "【シャッターチャンス！？】和泉愛依", idol: "和泉愛依", unit: "ストレイライト", },
        { rarity: "SSR", name: "【10個、光】浅倉透", idol: "浅倉透", unit: "ノクチル", },
        { rarity: "SSR", name: "【途方もない午後】浅倉透", idol: "浅倉透", unit: "ノクチル", },
        { rarity: "SSR", name: "【カラカラカラ】樋口円香", idol: "樋口円香", unit: "ノクチル", },
        { rarity: "SSR", name: "【ギンコ・ビローバ】樋口円香", idol: "樋口円香", unit: "ノクチル", },
        { rarity: "SSR", name: "【ポシェットの中には】福丸小糸", idol: "福丸小糸", unit: "ノクチル", },
        { rarity: "SSR", name: "【HAPPY-!NG】市川雛菜", idol: "市川雛菜", unit: "ノクチル", },
        { rarity: "SR", name: "【ナチュラルモード】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【ぐうぜんBOOKS】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【しじまに華ひととき】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【花結びゆくゆく】櫻木真乃", idol: "櫻木真乃", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【手作りの心遣い】風野灯織", idol: "風野灯織", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【マフラー直して、心は解けて】風野灯織", idol: "風野灯織", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【落下予測地点】風野灯織", idol: "風野灯織", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【インビジブルタイム】風野灯織", idol: "風野灯織", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【キネマ・リリックで夢見て】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【小さな夜のトロイメライ】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【チエルアルコは流星の】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【ハロー・ワールド】八宮めぐる", idol: "八宮めぐる", unit: "イルミネーションスターズ", },
        { rarity: "SR", name: "【ばってん長崎恋岬】月岡恋鐘", idol: "月岡恋鐘", unit: "アンティーカ", },
        { rarity: "SR", name: "【ビ～♡バップ海岸】月岡恋鐘", idol: "月岡恋鐘", unit: "アンティーカ", },
        { rarity: "SR", name: "【は〜と♡に火をつけて】月岡恋鐘", idol: "月岡恋鐘", unit: "アンティーカ", },
        { rarity: "SR", name: "【猫道カントリー】月岡恋鐘", idol: "月岡恋鐘", unit: "アンティーカ", },
        { rarity: "SR", name: "【裏腹あまのじゃく】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SR", name: "【夕つ方まみみチック】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SR", name: "【そのまみみ無気力につき】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SR", name: "【FANCY 24g】田中摩美々", idol: "田中摩美々", unit: "アンティーカ", },
        { rarity: "SR", name: "【秘めやかファンサービス】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SR", name: "【ふれあい、おもいあい】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SR", name: "【雪染めロマンティカ】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SR", name: "【真紅一輪】白瀬咲耶", idol: "白瀬咲耶", unit: "アンティーカ", },
        { rarity: "SR", name: "【カラフルメタモルフォーゼ】三峰結華", idol: "三峰結華", unit: "アンティーカ", },
        { rarity: "SR", name: "【雨色、上機嫌】三峰結華", idol: "三峰結華", unit: "アンティーカ", },
        { rarity: "SR", name: "【お願い、ただの少女がいい】三峰結華", idol: "三峰結華", unit: "アンティーカ", },
        { rarity: "SR", name: "【包・帯・組・曲】幽谷霧子", idol: "幽谷霧子", unit: "アンティーカ", },
        { rarity: "SR", name: "【伝・伝・心・音】幽谷霧子", idol: "幽谷霧子", unit: "アンティーカ", },
        { rarity: "SR", name: "【白・白・白・祈】幽谷霧子", idol: "幽谷霧子", unit: "アンティーカ", },
        { rarity: "SR", name: "【天・天・白・布】幽谷霧子", idol: "幽谷霧子", unit: "アンティーカ", },
        { rarity: "SR", name: "【マメ丸と一緒！】小宮果穂", idol: "小宮果穂", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【デコって！ハッピークッキー】小宮果穂", idol: "小宮果穂", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【キャッチ・ザ・フォール！】小宮果穂", idol: "小宮果穂", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【窓辺・サイレントタイム】小宮果穂", idol: "小宮果穂", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【純真チョコレート】園田智代子", idol: "園田智代子", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【よろしく♡めしませっ】園田智代子", idol: "園田智代子", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【錯覚・Darling】園田智代子", idol: "園田智代子", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【バッドガールの羽ばたき】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【オフデーゲーム】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【STARTRAIN】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【夏服にWIND YOU！】西城樹里", idol: "西城樹里", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【想ひいろは】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【凛凛、凛世】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【微熱風鈴】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【ふらここのうた】杜野凛世", idol: "杜野凛世", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【ストイックトレーニング】有栖川夏葉", idol: "有栖川夏葉", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【カトレアの花言葉】有栖川夏葉", idol: "有栖川夏葉", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【鳥籠をひらいて】有栖川夏葉", idol: "有栖川夏葉", unit: "放課後クライマックスガールズ", },
        { rarity: "SR", name: "【似合うかな？】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SR", name: "【甜花ちゃんといっしょ☆】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SR", name: "【ふたつの雨】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SR", name: "【見つけたい、だけど……】大崎甘奈", idol: "大崎甘奈", unit: "アルストロメリア", },
        { rarity: "SR", name: "【秘密のだらだらタイム】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SR", name: "【シンメトリー・プールサイド】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SR", name: "【氷上バンビーナ】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SR", name: "【お日様染めのマリーナ】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SR", name: "【ストライク狙って】大崎甜花", idol: "大崎甜花", unit: "アルストロメリア", },
        { rarity: "SR", name: "【マイフェイバリット】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SR", name: "【一夏一刻物語】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SR", name: "【よそゆき顔のセレナーデ】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SR", name: "【ワン・モア・フラワリング】桑山千雪", idol: "桑山千雪", unit: "アルストロメリア", },
        { rarity: "SR", name: "【さかさま世界】芹沢あさひ", idol: "芹沢あさひ", unit: "ストレイライト", },
        { rarity: "SR", name: "【かしゃーんとがらがら】芹沢あさひ", idol: "芹沢あさひ", unit: "ストレイライト", },
        { rarity: "SR", name: "【ザ・冬優子イズム】黛冬優子", idol: "黛冬優子", unit: "ストレイライト", },
        { rarity: "SR", name: "【水×天カイヤナイト】黛冬優子", idol: "黛冬優子", unit: "ストレイライト", },
        { rarity: "SR", name: "【気になる！？染めちゃう！？】和泉愛依", idol: "和泉愛依", unit: "ストレイライト", },
        { rarity: "SR", name: "【てへっ♪】和泉愛依", idol: "和泉愛依", unit: "ストレイライト", },
        { rarity: "SR", name: "【ご褒美、いるよね♪】和泉愛依", idol: "和泉愛依", unit: "ストレイライト", },
        { rarity: "SR", name: "【まわるものについて】浅倉透", idol: "浅倉透", unit: "ノクチル", },
        { rarity: "SR", name: "【＃SS】市川雛菜", idol: "市川雛菜", unit: "ノクチル", },
      ],
    };
  },
};
</script>
