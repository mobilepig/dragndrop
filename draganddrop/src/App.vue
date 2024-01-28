<template>
  <div class="container">
    <div class="wholeBox">
      <div
        class="first box"
        v-on:drop.prevent="onDrop($event, 'kart')"
        v-on:dragenter.prevent
        v-on:dragover.prevent
      >
        <h5>장바구니</h5>
        <div
          v-for="(item, idx) in data.kart"
          :key="idx"
          class="element"
          draggable="true"
          v-on:dragstart="startDrag($event, item, item.name)"
        >
          {{ item["name"] }}
        </div>
      </div>
      <div
        class="second box"
        v-on:drop.prevent="onDrop($event, 'plan')"
        v-on:dragenter.prevent
        v-on:dragover.prevent
      >
        <h5>동선짜기</h5>
        <div
          v-for="(item, idx) in data.plan"
          :key="idx"
          class="element"
          draggable="true"
          v-on:dragstart="startDrag($event, item, item.name)"
        >
          {{ item["name"] }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";

export default {
  setup() {
    const data = reactive({
      kart: [
        { name: "불국사" },
        { name: "돼지국밥 먹기" },
        { name: "뮤지엄" },
        { name: "온천가기" },
      ],
      plan: [],
    });

    const startDrag = (event, item) => {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("selectedItem", JSON.stringify(item));
    };

    const onDrop = (event, colNum) => {
      const selectedItem = event.dataTransfer.getData("selectedItem");
      const place = JSON.parse(selectedItem);

      if (colNum === "kart") {
        // plan에서 kart으로 옮길 때
        // place의 name이 이미 kart에 있으면 푸시안됨
        if (!data.kart.some((item) => item.name === place.name)) {
          data.kart.push(place);
          // 원래 배열에서 해당 아이템 삭제
          const index = data.plan.findIndex((item) => item.name === place.name);
          if (index !== -1) {
            data.plan.splice(index, 1);
          }
        }
      } else {
        // kart에서 plan로 옮길 때
        // place의 name이 이미 plan에 있으면 푸시안됨
        if (!data.plan.some((item) => item.name === place.name)) {
          data.plan.push(place);
          // 원래 배열에서 해당 아이템 삭제
          const index = data.kart.findIndex((item) => item.name === place.name);
          if (index !== -1) {
            data.kart.splice(index, 1);
          }
        }
      }
    };

    return {
      data,
      startDrag,
      onDrop,
    };
  },
};
</script>

<style lang="scss">
#app {
  width: 100vw;
}
body {
  margin: 0;
}
.container {
  display: flex;
  justify-content: center;
  width: 100%;
  margin: 0;
  padding: 0;

  background-color: #f9f7f3;
  .wholeBox {
    display: flex;
    width: 100%;
    .box {
      width: 50%;
      padding: 30px;
      height: 100vh;
      .element {
        display: flex;
        justify-content: center;
        width: 80%;
        background-color: #003566;
        color: white;
        margin: 10px;
        padding: 10px;
        border-radius: 5px;
      }
    }
    .first {
      background-color: #b5e2fa;
    }
    .second {
      background-color: #eddea4;
    }
  }
}
</style>
