<template>
  <v-app>
    <FormProduct></FormProduct>
    <Table :productData="productData"></Table>
  </v-app>
</template>

<script>
import Table from "./components/Table";
import FormProduct from "./components/FormProduct";

export default {
  name: "App",

  components: {
    Table,
    FormProduct
  },

  data: () => ({
    productData: []
  }),
  methods: {
    async startComponent() {
      try {
        const response = await fetch(
          "http://grizzly-dev.site/api/test/products"
        );
        const data = await response.json();
        this.productData = data;
      } catch (e) {
        console.error(e);
      }
    }
  },
  mounted: function() {
    this.startComponent();
  },
  watch: {
    productData: function() {
      this.startComponent();
    }
  }
};
</script>
