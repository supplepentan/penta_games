<script setup lang="ts">
const tiles = []; // タイル配列
const cardStyle =
  "w-36 h-36 border-4 border-black rounded-lg text-center text-3xl bg-gray-200"; //カードのスタイル
const moveCount = ref(0);
const isCleared = ref(false);
onMounted(() => {
  let table = document.getElementById("table"); // table要素の参照

  for (let i = 0; i < 4; i++) {
    // 4行分ループ
    let tr = document.createElement("tr"); // tr要素の作成
    for (let j = 0; j < 4; j++) {
      // 各列分ループ
      let td = document.createElement("td"); // td要素の作成
      let index = i * 4 + j;
      td.className = cardStyle; // class設定
      td.index = index; // タイルの並び順
      td.value = index; // 描画されている値
      td.textContent = index == 15 ? "" : index + 1; // 0は空欄に
      td.onclick = click; // クリック時のハンドラ登録
      tr.appendChild(td); // 行trに列tdを追加
      tiles.push(td);
    }
    table.appendChild(tr); // テーブルに行trを追加
  }

  for (let i = 0; i < 1000; i++) {
    // 10回、疑似的にランダムにクリックして並べ替え
    click({ target: { index: Math.floor(Math.random() * 16) } });
  }
  moveCount.value = 0;
  isCleared.value = false;
});

function init() {
  let table = document.getElementById("table"); // table要素の参照
  table.innerHTML = "";
  tiles.length = 0;

  for (let i = 0; i < 4; i++) {
    // 4行分ループ
    let tr = document.createElement("tr"); // tr要素の作成
    for (let j = 0; j < 4; j++) {
      // 各列分ループ
      let td = document.createElement("td"); // td要素の作成
      let index = i * 4 + j;
      td.className = "tile"; // class設定
      td.index = index; // タイルの並び順
      td.value = index; // 描画されている値
      td.textContent = index == 15 ? "" : index + 1; // 0は空欄に
      td.onclick = click; // クリック時のハンドラ登録
      tr.appendChild(td); // 行trに列tdを追加
      tiles.push(td);
    }
    table.appendChild(tr); // テーブルに行trを追加
  }

  for (let i = 0; i < 1000; i++) {
    // 10回、疑似的にランダムにクリックして並べ替え
    click({ target: { index: Math.floor(Math.random() * 16) } });
  }
  moveCount.value = 0;
  isCleared.value = false;
}

function click(e) {
  let i = e.target.index; // どの場所がクリックされたか

  if (i - 4 >= 0 && tiles[i - 4].value == 15) {
    swap(i, i - 4); // 上と入れ替え
  } else if (i + 4 < 16 && tiles[i + 4].value == 15) {
    swap(i, i + 4); // 下と入れ替え
  } else if (i % 4 != 0 && tiles[i - 1].value == 15) {
    swap(i, i - 1); // 左と入れ替え
  } else if (i % 4 != 3 && tiles[i + 1].value == 15) {
    swap(i, i + 1); // 右と入れ替え
  }
}

// i番目のタイルとj番目のタイルの番号を入れ替え
function swap(i: number, j: number) {
  let tmp = tiles[i].value; // 変更先を一時退避
  let x = tiles[i].value == 15 ? "" : tiles[i].value + 1;
  let y = tiles[j].value == 15 ? "" : tiles[j].value + 1;
  tiles[i].textContent = y;
  tiles[i].value = tiles[j].value;
  tiles[j].textContent = x;
  tiles[j].value = tmp;
  moveCount.value++;
  isCleared.value = tiles.every((t, idx) => t.value === idx);
}

function resetGame() {
  init();
}
</script>

<template>
  <div class="flex flex-col items-center py-2">
    <p class="mb-2">Moves: {{ moveCount }}</p>
    <table id="table"></table>
    <p v-if="isCleared" class="mt-2 text-green-600 font-bold">Cleared!</p>
    <button @click="resetGame" class="mt-2 px-4 py-1 bg-blue-300 rounded">
      Reset
    </button>
  </div>
</template>

<style scoped>
.tile {
  @apply w-36 h-36 border-4 border-black rounded-lg text-center text-3xl bg-gray-200;
}
</style>
