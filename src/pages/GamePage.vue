<template>
  <div id="gamePage">
    <a-row align="space-between">
      <a-button style="margin-bottom: 8px" @click="doBack"> 返回</a-button>
      <a-button>块数：{{ clearBlockNum }} / {{ totalBlockNum }}</a-button>
    </a-row>
    <!-- 胜利 -->
    <a-row align="center">
      <div v-if="gameStatus === 3" style="text-align: center">
        <h2>恭喜，你赢啦！🎉</h2>
        <img class="imgwin" src="../assets/chengcheng.jpg" />
      </div>
    </a-row>
    <!-- 输了再来 -->
    <a-row align="center">
      <div v-if="gameStatus === 2" style="text-align: center">
        <h2>可惜，你输了，再玩一次吧</h2>
        <img class="imglose" src="../assets/lose.jpg" />
      </div>
    </a-row>
    <!-- 分层选块 -->
    <a-row align="center">
      <div v-show="gameStatus < 2" class="level-board">
        <div v-for="(block, idx) in levelBlocksVal" :key="idx">
          <div
            v-if="block.status === 0"
            class="block level-block"
            :class="{
              disabled: !isHolyLight && block.lowerThanBlocks.length > 0,
            }"
            :data-id="block.id"
            :style="{
              zIndex: 100 + block.level,
              left: block.x * widthUnit + 'px',
              top: block.y * heightUnit + 'px',
            }"
            @click="() => doClickBlock(block)"
          >
              <img class="imgblock" :src="block.type">
          </div>
        </div>
      </div>
    </a-row>
    <!-- 随机选块 -->
    <a-row align="center">
      <div v-show="gameStatus < 2" class="random-board">
      <div
        v-for="(randomBlock, index) in randomBlocksVal"
        :key="index"
        class="random-area"
      >
        <div
          v-if="randomBlock.length > 0"
          :data-id="randomBlock[0].id"
          :style="{
              zIndex: 100,
              left: 200 * index + 'px'
            }"
          class="block level-block"
          @click="() => doClickBlock(randomBlock[0], index)"
        >
          <img class="imgblock" :src="randomBlock[0].type">
        </div>
        <!-- 隐藏 -->
        <div
          v-for="num in Math.max(randomBlock.length - 1, 0)"
          :key="num"
          :style="{
              zIndex: 100 - num,
              left: 200 * index + 10 * num + 'px'
            }"
          class="block level-block disabled"
        >
          <span v-if="canSeeRandom">
            <img class="imgblock" :src="randomBlock[num].type">
          </span>
        </div>
      </div>
      </div>
    </a-row>
    <!-- 槽位 -->
    <a-row v-if="slotAreaVal.length > 0" align="center" class="slot-board">
      <div v-for="(slotBlock, index) in slotAreaVal" :key="index" class="block">
         <img v-if="slotBlock?.type != null" class="imgblock" :src="slotBlock?.type">
      </div>
    </a-row>
    <!-- 技能 -->
    <div class="skill-board">
      <a-space>
        <a-button size="small" @click="doRevert">撤回</a-button>
        <a-button size="small" @click="doRemove">移出</a-button>
        <a-button size="small" @click="doShuffle">洗牌</a-button>
        <a-button size="small" @click="doBroke">破坏</a-button>
        <a-button size="small" @click="doHolyLight">圣光</a-button>
        <a-button size="small" @click="doSeeRandom">透视</a-button>
      </a-space>
    </div>
  </div>
</template>

<script setup lang="ts">
import useGame from "../core/game";
import { onMounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const {
  gameStatus,
  levelBlocksVal,
  randomBlocksVal,
  slotAreaVal,
  widthUnit,
  heightUnit,
  totalBlockNum,
  clearBlockNum,
  isHolyLight,
  canSeeRandom,
  doClickBlock,
  doStart,
  doShuffle,
  doBroke,
  doRemove,
  doRevert,
  doHolyLight,
  doSeeRandom,
} = useGame();

/**
 * 回上一页
 */
const doBack = () => {
  if (window.history.length > 1)
    router.back();
  else
    router.push("/");
};

onMounted(() => {
  doStart();
});
</script>

<style scoped>
.level-board {
  position: relative;
}

.level-block {
  position: absolute;
}

.random-board {
  margin-top: 8px;
  position: relative;
}

.random-area {
  margin-top: 8px;
}

.slot-board {
  border: 10px solid saddlebrown;
  margin: 5px auto;
  width: fit-content;
}

.skill-board {
  text-align: center;
}

.block {
  width: 45px;
  height: 45px;
  border: 1px solid #eee;
  background: white;
  text-align: center;
  vertical-align: top;
  display: inline-block;
}

.imgblock {
  width: 43px;
  height: 43px;
}
.imgwin {
  width: 100%;
}

.imglose {
  width: 100%;
}

.disabled {
  background: grey;
  cursor: not-allowed;
  filter: grayscale(100%) brightness(40%);
}
</style>

<img src="../assets/1.jpg" hidden>
<img src="../assets/2.jpg" hidden>
<img src="../assets/3.jpg" hidden>
<img src="../assets/4.jpg" hidden>
<img src="../assets/5.jpg" hidden>
<img src="../assets/6.jpg" hidden>
<img src="../assets/7.jpg" hidden>
<img src="../assets/8.jpg" hidden>
<img src="../assets/9.jpg" hidden>
<img src="../assets/10.jpg" hidden>
<img src="../assets/11.jpg" hidden>
<img src="../assets/12.jpg" hidden>
<img src="../assets/13.jpg" hidden>
<img src="../assets/14.jpg" hidden>
<img src="../assets/15.jpg" hidden>
<img src="../assets/16.jpg" hidden>
<img src="../assets/17.jpg" hidden>
