<template>
  <div class="eight-queen-container">
    <div class="row-wrapper" v-for="(item, rowIndex) in grids" :key="rowIndex">
      <div
        class="cell"
        v-for="(colItem, colIndex) in item"
        :key="colIndex"
        @click="selectCell(rowIndex, colIndex)"
      >
        <span class="selected-icon" v-if="colItem.isOk">Q</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EightQueen",
  data() {
    return {
      grids: [],
    };
  },
  created() {
    this.initGrids();
  },
  methods: {
    initGrids() {
      const grids = [];
      new Array(8).fill(1).forEach((row, rowIndex) => {
        const rowArr = [];
        new Array(8).fill(1).forEach((col, colIndex) => {
          rowArr.push({
            key: rowIndex * 8 + colIndex,
            isOk: false,
          });
        });
        grids.push(rowArr);
      });
      this.grids = grids;
    },
    selectCell(rowIndex, colIndex) {
      if (this.grids[rowIndex][colIndex].isOk) {
        this.grids[rowIndex][colIndex].isOk = false;
      } else if (this.isValid(rowIndex, colIndex)) {
        this.grids[rowIndex][colIndex].isOk = true;
      } else {
        alert("当前位置不能放Queen");
      }
    },
    isValid(rowIndex, colIndex) {
      // 1.判断行上是否存在
      if (
        this.grids[rowIndex].some((item) => {
          return item.isOk;
        })
      )
        return false;
      // 2.判断列上是否存在
      if (
        this.grids.some((row) => {
          return row[colIndex].isOk;
        })
      )
        return false;
      // 3.撇
      for (let i = 0; i < this.grids[0].length; i++) {
        let y = rowIndex + colIndex - i;
        if (y >= 0 && y < this.grids.length && this.grids[y][i].isOk)
          return false;
      }
      // 4.捺
      for (let i = 0; i < this.grids[0].length; i++) {
        let y = rowIndex - colIndex + i;
        if (y >= 0 && y < this.grids.length && this.grids[y][i].isOk)
          return false;
      }
      return true;
    },
  },
};
</script>

<style scoped>
.cell {
  width: 40px;
  height: 40px;
  background-color: aliceblue;
  float: left;
  display: flex;
  align-items: center;
  justify-content: center;
}
.cell:nth-child(2n) {
  background-color: bisque;
}
.row-wrapper {
  width: 320px;
  height: 40px;
}
.row-wrapper:nth-child(2n) .cell:nth-child(2n-1) {
  background-color: bisque;
}
.row-wrapper:nth-child(2n) .cell:nth-child(2n) {
  background-color: aliceblue;
}
.selected-icon {
  font-size: 12px;
}
</style>
