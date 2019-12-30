<template>
  <div class="divide-box">
    <table cellpadding="0" cellspacing="0">
      <tr class="top">
        <td colspan="4">
          <table>
            <tr>
              <td class="title">
                <img
                  src="https://static.mundoeducacao.bol.uol.com.br/mundoeducacao/conteudo_legenda/fd9730ac7f1000d14c069d39e23daacb.jpg"
                  style="width:100%; max-width:300px;"
                />
              </td>

              <td>
                <b>Calculadora</b>
                <br /> Dividir conta
              </td>
            </tr>
          </table>
        </td>
      </tr>
      <tr class="heading">
        <td colspan="4">Quantas pessoas?</td>
      </tr>
      <tr class="details">
        <td colspan="4">
          <input type="number" v-model="peopleQtd"/>
        </td>
      </tr>
      <br />
      <tr class="heading">
        <td>Item</td>
        <td>Preço</td>
        <td>Quantidade</td>
        <td>Total</td>
      </tr>
      <tr class="item" v-for="item in items" :key="item.id">
        <td>
          <input v-model="item.description" />
        </td>
        <td>
          R$
          <input type="number" v-model="item.price" />
        </td>
        <td>
          <input type="number" v-model="item.quantity" />
        </td>
        <td>R$ {{item.price * item.quantity | currency}}</td>
      </tr>
      <tr>
        <td colspan="4">
          <button class="btn-add-row" @click="addRow">Adicionar item</button>
        </td>
      </tr>
      <tr class="heading">
        <td colspan="4">Taxa de serviço</td>
      </tr>
      <tr class="details">
        <td colspan="3">
          <input type="checkbox" v-model="hasServiceTax"/>
          <input type="number" v-model="serviceTaxValue"/>
        </td>
        <td>R$ {{calcTax}}</td>
      </tr>
      <tr class="total">
        <td></td>
        <td colspan="3">Total: R$ {{ total | currency}}</td>
      </tr>
      <tr class="total">
        <td></td>
        <td colspan="3">R$ {{ totalDivide | currency}} dividido para cada pessoa</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "DivideBill",
  data() {
    return {
      peopleQtd: 1,
      hasServiceTax: true,
      serviceTaxValue: 10.00,
      items: [
        { id: 1, description: "Coca Cola", quantity: 3, price: 5 },
        { id: 2, description: "Almoço", quantity: 3, price: 25 },
        { id: 3, description: "Sobremesa", quantity: 2, price: 7 },
      ]
    };
  },
  computed: {
    total() {
      return this.items.reduce(
        (acc, item) => acc + item.price * item.quantity, 0
      ) + this.calcTax;
    },
    totalDivide() {
      if (this.peopleQtd >= 1)
        return this.total / this.peopleQtd;
      else
        return this.total;
    },
    calcTax() {
      let total = this.items.reduce(
        (acc, item) => acc + item.price * item.quantity, 0
      );
      if (this.hasServiceTax)
        return total * this.serviceTaxValue / 100;
      return 0;
    }
  },
  methods: {
    addRow() {
      this.items.push({
        description: "",
        quantity: 1,
        price: 0
      });
    }
  },
  filters: {
    currency(value) {
      return value.toFixed(2);
    }
  }
};
</script>

<style scoped>
.divide-box {
  max-width: 800px;
  margin: auto;
  padding: 30px;
  border: 1px solid #eee;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.15);
  font-size: 16px;
  line-height: 24px;
  font-family: "Helvetica Neue", "Helvetica", Helvetica, Arial, sans-serif;
  color: #555;
}

.divide-box table {
  width: 100%;
  line-height: inherit;
  text-align: left;
}

.divide-box table td {
  padding: 5px;
  vertical-align: top;
}

.divide-box table tr td:nth-child(2) {
  text-align: right;
}

.divide-box table tr.top table td {
  padding-bottom: 20px;
}

.divide-box table tr.top table td.title {
  font-size: 45px;
  line-height: 45px;
  color: #333;
}

.divide-box table tr.heading td {
  background: #eee;
  border-bottom: 1px solid #ddd;
  font-weight: bold;
}

.divide-box table tr.details td {
  padding-bottom: 20px;
}

.divide-box table tr.item td {
  border-bottom: 1px solid #eee;
  line-height: 34px;
}

.divide-box table tr.item td input {
  height: 22px;
  border-radius: 5px;
  border: solid 1px #ccc;
  padding: 5px;
}

.divide-box table tr.item.last td {
  border-bottom: none;
}

.divide-box table tr.total td:nth-child(2) {
  border-top: 2px solid #eee;
  font-weight: bold;
}

@media only screen and (max-width: 600px) {
  .divide-box table tr.top table td {
    width: 100%;
    display: block;
    text-align: center;
  }
}

/** RTL **/
.rtl {
  direction: rtl;
  font-family: Tahoma, "Helvetica Neue", "Helvetica", Helvetica, Arial,
    sans-serif;
}

.rtl table {
  text-align: right;
}

.rtl table tr td:nth-child(2) {
  text-align: left;
}
</style>
