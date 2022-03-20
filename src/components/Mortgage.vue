<template>
  <div>
    <h2 class="text-xl">Calculette prêt immobilier {{ startprice }}</h2>
    <div class="flex flex-wrap flex-col md:flex-row">
      <form class="flex-1 p-8 bg-secondary card card-bordered m-6">
        <ul>
          <li>
            <div class="form-control">
              <label class="input-group input-group-sm">
                <span>Acheteur</span>
                <input
                  type="number"
                  v-model="acheteur"
                  class="input input-sm w-full"
                />
                <span>per.</span>
              </label>
            </div>
          </li>
          <li>
            <div class="form-control">
              <label class="input-group input-group-sm">
                <span>Prix</span>
                <input
                  type="number"
                  v-model="price"
                  class="input input-sm w-full"
                />
                <span>EUR</span>
              </label>
              <input
                type="range"
                max="990000"
                v-model="price"
                class="range range-xs"
              />
            </div>
          </li>
          <li></li>
          <li>
            <div class="form-control">
              <label class="input-group input-group-sm">
                <span>Frais</span>
                <input
                  type="number"
                  v-model="taxes"
                  class="input input-sm w-full"
                />
                <span>%</span>
              </label>
            </div>
          </li>
          <li>
            <div class="form-control">
              <label class="input-group input-group-sm">
                <span>Apport</span>
                <input
                  type="number"
                  v-model="reserve"
                  class="input input-sm w-full"
                />
                <span>EUR</span>
              </label>
            </div>
          </li>
          <li>
            <div class="form-control">
              <label class="input-group input-group-sm">
                <span>Taux</span>
                <input
                  type="number"
                  v-model="loanrate"
                  class="input input-sm w-full"
                />
                <span>%</span>
              </label>
            </div>
          </li>
          <li>
            <div class="form-control">
              <label class="input-group input-group-sm">
                <span>Durée</span>
                <input
                  type="number"
                  v-model="length"
                  class="input input-sm w-full"
                />
                <span>ans</span>
              </label>
            </div>
          </li>
        </ul>
      </form>
      <div
        id="simresult"
        class="flex-1 p-8 bg-secondary card card-bordered m-6"
      >
        <div class="flex justify-between">
          <span>Prix de vente</span>
          <span>
            {{
              price.toLocaleString("fr-BE", {
                style: "currency",
                currency: "EUR",
              })
            }}
          </span>
        </div>
        <div class="flex justify-between">
          <span>Prix total avec frais</span>
          <span>
            {{
              prixtotal.toLocaleString("fr-BE", {
                style: "currency",
                currency: "EUR",
              })
            }}
          </span>
        </div>
        <div class="flex justify-between">
          <span>Frais</span>
          <span>
            {{
              frais.toLocaleString("fr-BE", {
                style: "currency",
                currency: "EUR",
              })
            }}
          </span>
        </div>
        <div class="flex justify-between">
          <span>Apport</span>
          <span>
            {{
              reserve.toLocaleString("fr-BE", {
                style: "currency",
                currency: "EUR",
              })
            }}
          </span>
        </div>
        <div class="flex justify-between">
          <span>Prêt total</span>
          <span>
            {{
              financable.toLocaleString("fr-BE", {
                style: "currency",
                currency: "EUR",
              })
            }}
          </span>
        </div>
        <div class="flex justify-between">
          <span>Quotité emprunt</span>
          <span
            >{{ 100 - (100 / (price / (reserve - frais))).toFixed(2) }} %</span
          >
        </div>
        <div class="flex justify-between">
          <span>Quotité emprunt sur total</span>
          <span>{{ 100 - (100 / (prixtotal / reserve)).toFixed(2) }} %</span>
        </div>
        <div class="flex justify-between">
          <span>Cout du prêt</span>
          <span>
            {{
              (mensualite * length * 12 - financable).toLocaleString("fr-BE", {
                style: "currency",
                currency: "EUR",
              })
            }}
          </span>
        </div>
        <div class="mt-4">
          Mensualités sur <span class="badge">{{ length }}</span> ans est de
          <span class="badge">{{
            parseFloat(mensualite).toLocaleString("fr-BE", {
              style: "currency",
              currency: "EUR",
            })
          }}</span>
          pour un taux de
          <span class="badge">{{ loanrate }} %</span>
        </div>
        <div>
          <span class="badge font-bold"
            >Mensualité par personne
            {{
              (mensualite / acheteur).toLocaleString("fr-BE", {
                style: "currency",
                currency: "EUR",
              })
            }}</span
          >
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import { ref, computed } from "vue";
  //import { VNumber } from "vue-number-format";

  export default {
    /*  props: {
      startprice: {
        type: Number,
        default: 300000,
      },
    },*/
    data() {
      return {
        number: {
          decimal: ".",
          separator: ",",
          prefix: "$ ",
          suffix: " #",
          precision: 2,
          masked: false /* doesn't work with directive */,
        },
      };
    },
    setup() {
      //<-(props)
      const acheteur = ref(2);
      const price = ref(295000);
      const taxes = ref(1.16);
      const reserve = ref(106000);
      const length = ref(15);
      const loanrate = ref(1.45);
      const prixtotal = computed(
        () => parseFloat(price.value) * parseFloat(taxes.value)
      );
      const frais = computed(
        () => parseFloat(price.value) * parseFloat(taxes.value - 1)
      );
      const financable = computed(
        () =>
          parseFloat(price.value) -
          (parseFloat(reserve.value) - parseFloat(frais.value))
      );
      const inter1 = computed(
        () => parseFloat(financable.value) * (loanrate.value / 100 / 12)
      );

      const inter2 = ref(
        1 - Math.pow(1 + loanrate.value / 12, -length.value * 12)
      );

      const mensualite = computed(
        () =>
          (parseFloat(financable.value) * (loanrate.value / 100 / 12)) /
          (1 - Math.pow(1 + loanrate.value / 100 / 12, -length.value * 12))
      );

      /*onlyForCurrency ($event) {
     // console.log($event.keyCode); //keyCodes value
     let keyCode = ($event.keyCode ? $event.keyCode : $event.which);

     // only allow number and one dot
     if ((keyCode < 48 || keyCode > 57) && (keyCode !== 46 || this.price.indexOf('.') != -1)) { // 46 is dot
      $event.preventDefault();
     }

     // restrict to 2 decimal places
     if(this.price!=null && this.price.indexOf(".")>-1 && (this.price.split('.')[1].length > 1)){
     $event.preventDefault();
     }
   }*/

      return {
        price,
        mensualite,
        taxes,
        reserve,
        length,
        loanrate,
        prixtotal,
        financable,
        inter1,
        inter2,
        frais,
        acheteur,
      };
    }, //setup
  };
</script>
<style scoped>
  h2 {
    @apply text-2xl font-bold;
  }

  label > span {
    min-width: 6em;
    font-weight: bold;
  }

  li {
    margin-bottom: 9px;
  }

  #simresult {
    font-weight: 600;
  }
</style>
