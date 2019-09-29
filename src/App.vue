<template>
  <body
    id="app"
    class="app"
    :class="{ 'app--popup-open': popupOpen, 'app--dark': isDarkMode }"
  >
    <noscript>
      <strong
        >We're sorry but coffee-island-vue doesn't work properly without
        JavaScript enabled. Please enable it to continue.</strong
      >
    </noscript>
    <div class="theme-selector" @click="changeTheme">
      <svg
        class="theme-selector__icon"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 16 16"
      >
        <path
          d="M8,0C3.582,0,0,3.582,0,8s3.582,8,8,8s8-3.582,8-8S12.418,0,8,0z M8,14c-3.314,0-6-2.686-6-6
				s2.686-6,6-6s6,2.686,6,6S11.314,14,8,14z"
        />
        <path d="M8,3v10c2.757,0,5-2.243,5-5S10.757,3,8,3z" />
      </svg>
    </div>
    <Receipt
      :receiptData="{
        drinks: Number(receiptDataProps.drinks),
        other: Number(receiptDataProps.other)
      }"
      @boom="
        {
          togglePopup;
        }
      "
    />
    <BasePopup :isOpen="popupOpen">
      <label for="drinks">
        <span>Drinks</span>
        <input
          type="number"
          name="drinks"
          id="drinks"
          step="0.01"
          min="0"
          v-model="receiptDataProps.drinks"
        />
      </label>
      <label for="other">
        <span>Other</span>
        <input
          type="number"
          name="other"
          id="other"
          step="0.01"
          min="0"
          v-model="receiptDataProps.other"
        />
      </label>
      <BaseButton @boom="togglePopup">
        Απόδειξη!
      </BaseButton>
    </BasePopup>
  </body>
</template>

<script>
import BasePopup from "./components/BasePopup";
import BaseButton from "./components/BaseButton";
import Receipt from "./components/Receipt";

export default {
  name: "app",
  components: {
    BasePopup,
    BaseButton,
    Receipt
  },
  data() {
    return {
      popupOpen: true,
      receiptDataProps: {
        drinks: 0,
        other: 0
      },
      isDarkMode: window.localStorage.getItem("darkMode") === "true"
    };
  },
  methods: {
    togglePopup() {
      console.log("TogglingPopup!");
      this.popupOpen = !this.popupOpen;
    },
    calculateReceipt() {
      console.log("calculating receipt");
    },
    changeTheme() {
      window.localStorage.setItem(
        "darkMode",
        window.localStorage.getItem("darkMode") === "true" ? false : true
      );
      this.isDarkMode = !this.isDarkMode;
    }
  },
  created() {
    window.localStorage.setItem("darkMode", this.isDarkMode);
  }
};
</script>

<style lang="scss">
@import "./scss/variables";
@import "~sanitize.css/sanitize.css";
@import "./scss/browser-generic";
@import "./scss/theme-selector";
@import "./scss/buttons";
@import "./scss/popup";
</style>
