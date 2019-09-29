<template>
  <main class="main">
    <section class="receipt">
      <header class="receipt-header">
        <img :src="peinataLogo" alt="" />
        <time>{{ formatedDate }}</time>
      </header>
      <hr />
      <div class="receipt__body">
        <div class="row">
          <div class="col">Ροφήματα</div>
          <div class="col">{{ receiptData.drinks }}€</div>
        </div>
        <div class="row">
          <div class="col">Άλλα</div>
          <div class="col">{{ receiptData.other }}€</div>
        </div>
        <hr class="dashed" />
        <div class="row row--highlighted">
          <div class="col">Σύνολο</div>
          <div class="col">{{ originalTotal }}€</div>
        </div>
        <template v-if="peinata">
          <div class="row">
            <div class="col">Έκπτωση Πεινάτας</div>
            <div class="col">{{ peinata }}€</div>
          </div>
          <hr />
          <div class="row row--highlighted">
            <div class="col">Νέα τιμή ροφημάτων</div>
            <div class="col">{{ updatedDrinksPrice }}€</div>
          </div>
          <div class="row row--highlighted">
            <div class="col">Νέα τιμή διάφορων</div>
            <div class="col">{{ updatedOtherPrice }}€</div>
          </div>
          <hr class="dashed" />
          <div class="row row--highlighted">
            <div class="col">Νέο Σύνολο</div>
            <div class="col">{{ updatedTotal }}€</div>
          </div>
        </template>
      </div>
    </section>
    <section class="receipt-footer">
      <BaseButton @click="$emit('boom')">Νέα απόδειξη!</BaseButton>
      <a href="http://holy.gd" class="holy-link">
        <img :src="holyLogo" alt="" />
      </a>
    </section>
  </main>
</template>

<script>
import BaseButton from "./BaseButton";
import peinataLogo from "../assets/joker-icon.png";
import holyLogo from "../assets/holy.svg";
import moment from "moment";
import "moment/locale/el";

export default {
  name: "Receipt",
  components: {
    BaseButton
  },
  props: {
    receiptData: Object
  },
  data() {
    return {
      peinataLogo: peinataLogo,
      holyLogo: holyLogo
      // receiptData: this.receiptDataProps
    };
  },
  computed: {
    formatedDate() {
      return moment()
        .locale("el")
        .format("D MMMM YYYY, H:mm:ss");
    },
    originalTotal() {
      return this.receiptData.drinks + this.receiptData.other;
    },
    peinata() {
      if (this.originalTotal >= 20) {
        return 7;
      } else if (this.originalTotal >= 15) {
        return 5;
      } else if (this.originalTotal >= 6) {
        return 2;
      } else {
        return undefined;
      }
    },
    drinksPercentage() {
      return ((this.receiptData.drinks / this.originalTotal) * 100).toFixed(2);
    },
    drinksPricePortionOnPeinata() {
      return ((this.drinksPercentage / 100) * this.peinata).toFixed(2);
    },
    updatedDrinksPrice() {
      return (
        this.receiptData.drinks - this.drinksPricePortionOnPeinata
      ).toFixed(2);
    },
    otherPercentage() {
      return ((this.receiptData.other / this.originalTotal) * 100).toFixed(2);
    },
    otherPricePortionOnPeinata() {
      return ((this.otherPercentage / 100) * this.peinata).toFixed(2);
    },
    updatedOtherPrice() {
      return (this.receiptData.other - this.otherPricePortionOnPeinata).toFixed(
        2
      );
    },
    updatedTotal() {
      return (
        parseFloat(this.updatedOtherPrice) + parseFloat(this.updatedDrinksPrice)
      );
    }
  },
  created() {
    // console.log(this.receiptDataProps);
  }
};
</script>

<style lang="scss">
main {
  padding: 1rem;
}

.receipt {
  max-width: 20rem;
  width: 100%;
  margin: 0 auto;
  padding: 2rem 1.5rem;
  box-shadow: 0 0 2rem rgba(0, 0, 0, 0.05);
  text-align: center;
  background: var(--background);
  border-radius: 0.5rem;

  &-header {
    > * {
      display: inline-block;

      + * {
        margin-top: 1.5rem;
      }
    }
  }

  &-footer {
    max-width: 20rem;
    width: 100%;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}

.row {
  text-align: left;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 0;

  &:last-child {
    padding-bottom: 0;
  }

  &--highlighted {
    font-weight: bolder;

    &:last-child {
      color: var(--accent);
    }
  }
}
</style>
