<template>
  <div class="container">
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">Id</th>
            <th class="text-left">Code</th>
            <th class="text-left">Name</th>
            <th class="text-left">Price</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(product, index) in productData.data"
            :key="index"
            @click="getInfoProduct(product)"
          >
            <td>{{ product.id }}</td>
            <td>{{ product.code }}</td>
            <td>{{ product.name }}</td>
            <td>{{ product.price }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
    <v-card class="product-card">
      <v-card-title> Название продукта: {{ name }} </v-card-title>
      <v-card-subtitle>Код продукта: {{ code }}</v-card-subtitle>
      <v-card-text>Цена продукта: {{ price }}</v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  props: {
    productData: [Array, Object]
  },
  data() {
    return {
      name: "",
      code: "",
      price: ""
    };
  },
  methods: {
    getInfoProduct(info) {
      this.name = info.name;
      this.code = info.code;
      this.price = info.price;
    }
  }
};
</script>

<style>
tr {
  cursor: pointer;
}
.product-card {
  margin-top: 30px;
}
</style>
