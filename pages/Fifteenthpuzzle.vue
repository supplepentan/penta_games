<script setup lang="ts">
const tiles = []; // タイル配列
const cardStyle =
  "w-36 h-36 border-4 border-black rounded-lg text-center text-3xl bg-gray-200"; //カードのスタイル
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
});

function init() {
  let table = document.getElementById("table"); // table要素の参照

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
}
</script>

<template>
  <div class="flex justify-center py-2">
    <table id="table"></table>
  </div>
</template>

<style scoped>
.tile {
  @apply w-36 h-36 border-4 border-black rounded-lg text-center text-3xl bg-gray-200;
}
</style>
