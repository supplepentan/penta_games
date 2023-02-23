<script setup>
useHead({
  title: "Flip Cards",
  meta: [
    { charset: "utf-8" },
    { name: "description", description: "Penta Games" },
    { name: "viewport", content: "width=device-width, initial-scale=1" },
  ],
  link: [
    {
      rel: "icon",
      sizes: "32x32",
      type: "image/png",
      href: "/favicon.ico",
    },
  ],
});
// 配列シャッフル
Array.prototype.shuffle = function () {
  let i = this.length;
  while (i) {
    let j = Math.floor(Math.random() * i);
    let t = this[--i];
    this[i] = this[j];
    this[j] = t;
  }
  return this;
};

let timer = NaN; // クリアまでの時間計測用タイマー
let flipTimer = NaN; // 裏に戻すためのタイマー
let score = 0; // スコア
let prevCard = null; // 1枚目に裏返したカード
let startTime = null; // ゲーム開始時刻

//css
const tdCard = "w-36 h-36 border-2 text-center text-2xl";
const tdCardBack = "w-36 h-36 bg-[url('~/assets/image/flipcards/card.png')]";
const tdRed = "bg-red-200";
const tdBlack = "bg-black";
// 初期化関数
onMounted(() => {
  let table = document.getElementById("table"); // tableへの参照取得

  let cards = []; // カード格納用配列
  let mark = ["♥", "♦", "♠", "♧"];
  for (let j = 0; j < 4; j++) {
    for (let i = 0; i < 13; i++) {
      cards.push(mark[j] + " " + (i + 1));
    }
  }
  cards.shuffle(); // カードをシャッフル
  console.log(cards);
  for (let i = 0; i < 4; i++) {
    let tr = document.createElement("tr"); // 行tr作成
    for (let j = 0; j < 13; j++) {
      let td = document.createElement("td"); // 列td作成
      td.className = tdCard + tdCardBack;
      td.number = cards[i * 13 + j];
      td.onclick = flip; // クリック時のハンドラ登録
      tr.appendChild(td); // 列tdを行trに追加
    }
    table.appendChild(tr); // 行trをtableに追加
  }

  startTime = new Date(); // ゲーム開始時刻を保存
  timer = setInterval(tick, 1000); // タイマー開始
});

// 経過時間計測用タイマー（１秒毎に実行）
function tick() {
  let now = new Date();
  let elapsed = Math.floor((now.getTime() - startTime.getTime()) / 1000);
  document.getElementById("time").textContent = elapsed; // 経過時刻を表示
}

// カード裏返し
function flip(e) {
  let src = e.target; // クリックされた要素
  if (flipTimer || src.textContent != "") {
    return; // 既に２枚反転 or 反転済のカードクリック時は何もしない
  }

  // class属性を設定して表面に
  let num = src.number;
  if (num.charAt(0) == "♥" || num.charAt(0) == "♦") {
    src.className = tdCard + tdRed;
  } else {
    src.className = tdCard + tdBlack;
  }
  src.textContent = num; // カードの数字を表示

  // １枚目の時は、それを記録して関数を抜ける
  if (prevCard == null) {
    prevCard = src;
    return;
  }

  // ２枚目 - カード一致判定
  let n0 = prevCard.number.substring(2);
  let n1 = num.substring(2);
  if (n0 == n1) {
    if (++score == 2 * 13) {
      clearInterval(timer); // すべて揃ったらタイマーを止める
    }
    prevCard = null;
    clearTimeout(flipTimer); // 裏返すタイマーを止める
  } else {
    // カード不一致の場合は1秒後にカード2枚を裏返しにする
    flipTimer = setTimeout(function () {
      src.className = tdCard + tdCardBack;
      src.textContent = "";
      prevCard.className = tdCard + tdCardBack;
      prevCard.textContent = "";
      prevCard = null;
      flipTimer = NaN;
    }, 1000);
  }
}
</script>
<template>
  <div>
    <table id="table"></table>
    <h2>経過時間：<span id="time">0</span>秒</h2>
  </div>
</template>
<style scoped>
td.card {
  width: 50px;
  height: 70px;
  border: 1px solid blue;
  border-radius: 10px;
  text-align: center;
  font-size: 20px;
  background-color: white;
  box-shadow: rgb(128, 128, 128) 5px 5px;
}
td.back {
  background-image: url("~/assets/images/flipcardscard.png");
  background-size: 50px 70px;
}
td.red {
  color: red;
}
td.black {
  color: black;
}
</style>
