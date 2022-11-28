<template>
  <div class="calc">
    <!-- selected sign container -->
    <div class="signContainer">
      <p v-if="selectedSign">{{ selectedSign }}</p>
    </div>
    <!-- result -->
    <div class="result_Container">
      <h2 class="result_text" v-text="rhsValue ? rhsValue : lhsValue"></h2>
    </div>
    <!-- keys -->
    <div class="keys_container">
      <template v-for="key in calcKeys" :key="key">
        <button
          :class="`key_btn display-flex ${
            key === '=' ? 'equalSign' : signs.includes(key) ? 'key_sign' : ''
          }`"
          @click="keyPressHandler(key)"
        >
          <span class="key_inner-container display-flex">
            {{ key }}
          </span>
          <span class="overlay" />
        </button>
      </template>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
const signs = ["x", "+", "-", "÷"];
const lhsValue = ref(0);
const rhsValue = ref(0);
const selectedSign = ref("");

const calcKeys = ref([
  "CE",
  ...signs,
  "1",
  "2",
  "3",
  "4",
  "5",
  "6",
  "7",
  "8",
  "9",
  "0",
  "=",
  ".",
]);

const keyPressHandler = (key: string) => {
  const signKeys = ["CE", "=", ...signs];
  if (!signKeys.includes(key) && selectedSign.value.length === 0) {
    /// updating left-hand-side value
    lhsValue.value = +`${lhsValue.value}${key}`;
  } else if (!signKeys.includes(key) && selectedSign.value.length > 0) {
    /// updating left-hand-side value
    rhsValue.value = +`${rhsValue.value}${key}`;
  } else if (key === "CE") {
    /// clearing vlaues
    lhsValue.value = 0;
    rhsValue.value = 0;
    selectedSign.value = "";
  } else if (key !== "CE" && key !== "=") {
    /// selecting the sign(+,-,/,*)
    selectedSign.value = key;
  } else if (key === "=") {
    /// calc the final output
    if (selectedSign.value === "+") {
      lhsValue.value = lhsValue.value + rhsValue.value;
      rhsValue.value = 0;
    } else if (selectedSign.value === "-") {
      lhsValue.value = lhsValue.value - rhsValue.value;
      rhsValue.value = 0;
    } else if (selectedSign.value === "*") {
      lhsValue.value = lhsValue.value * rhsValue.value;
      rhsValue.value = 0;
    } else if (selectedSign.value === "÷") {
      lhsValue.value = lhsValue.value / rhsValue.value;
      rhsValue.value = 0;
    }
    selectedSign.value = "";
  }
};
</script>

<style scoped>
.display-flex {
  display: flex;
  justify-content: center;
  align-items: center;
}
.calc {
  width: 558px;
  height: 632px;
  padding: 20px;
  padding-top: 80px;
  background-image: linear-gradient(to right bottom, #1f1f1f, #1e1e1e);
  border-radius: 20px;
  border: 5px solid #d4d0cc;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
}
.signContainer {
  width: 20px;
  position: absolute;
  right: 5%;
  top: 5%;
  font-size: 25px;
  text-align: center;
}

.result_Container {
  text-align: end;
}
.result_text {
  color: #eae9e7;
  font-size: 50px;
  font-weight: 400;
  font-family: serif;
}
.keys_container {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 1px;
}
.key_btn {
  font-size: 20px;
  height: 100px;
  border-radius: 10px;
  background-color: #d4d0cc;
  cursor: pointer;
  padding: 0;
  overflow: hidden;
  position: relative;
}
.key_inner-container {
  width: 70%;
  height: 70%;
  pointer-events: none;
  border-radius: 40px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4),
    -3px -3px 8px rgb(238, 238, 238, 0.5);
  background-color: rgba(0, 0, 0, 0.1);
}
.overlay {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  pointer-events: none;
  transition: all 0.1s;
}

.key_btn:hover .overlay {
  background-color: rgba(0, 0, 0, 0.1);
}
.equalSign {
  grid-column: 1/5;
  background-color: #d64728;
  color: #fff;
}
.equalSign .key_inner-container {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4),
    -3px -3px 8px rgb(238, 238, 238, 0.5);
}
.key_sign {
  background-color: #80807e;
  color: #fff;
}
</style>
