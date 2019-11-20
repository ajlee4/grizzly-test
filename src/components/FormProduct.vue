<template>
  <div>
    <v-form v-model="valid" @submit.prevent="addProduct" ref="form">
      <v-container>
        <v-row>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="code"
              :rules="codeRules"
              label="Code"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field
              v-model="name"
              :rules="nameRules"
              label="Name"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field
              v-model="price"
              :rules="priceRules"
              label="Price"
              required
            ></v-text-field>
          </v-col>
          <div class="form-btn">
            <v-btn
              :disabled="!valid"
              color="success"
              class="mr-4"
              type="submit"
            >
              POST
            </v-btn>
          </div>
        </v-row>
      </v-container>
    </v-form>
    <v-form
      v-model="deleteValid"
      @submit.prevent="deleteProduct()"
      ref="deleteForm"
    >
      <v-container>
        <v-row class="center">
          <v-col cols="12" md="4">
            <v-text-field
              v-model="id"
              label="Удалить элемент по ID"
              :rules="priceRules"
              required
            ></v-text-field>
          </v-col>
          <div class="form-btn">
            <v-btn
              color="error"
              class="mr-4"
              type="submit"
              :disabled="!deleteValid"
              :rules="priceRules"
              required
            >
              DELETE
            </v-btn>
          </div>
        </v-row>
      </v-container>
    </v-form>
    <v-form @submit.prevent="putProduct" v-model="putValid" ref="putForm">
      <v-container>
        <v-row>
          <v-col cols="12" md="3">
            <v-text-field
              v-model="putId"
              label="Изменить элемент по ID"
              :rules="priceRules"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field
              v-model="putCode"
              :rules="codeRules"
              label="Code"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="3">
            <v-text-field
              v-model="putName"
              :rules="nameRules"
              label="Name"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="3">
            <v-text-field
              v-model="putPrice"
              :rules="priceRules"
              label="Price"
              required
            ></v-text-field>
          </v-col>
          <div class="form-btn">
            <v-btn
              color="primary"
              class="mr-4"
              type="submit"
              :disabled="!putValid"
            >
              PUT
            </v-btn>
          </div>
        </v-row>
      </v-container>
    </v-form>
  </div>
</template>
<script>
export default {
  data: () => ({
    valid: false,
    deleteValid: false,
    putValid: false,
    name: "",
    code: "",
    price: "",
    putName: "",
    putCode: "",
    putPrice: "",
    putId: "",
    id: "",
    codeRules: [
      v => !!v || "Введите минимум 4 символа",
      v => v.length >= 4 || "Введите минимум 4 символа"
    ],
    nameRules: [
      v => !!v || "Введите минимум 3 символа",
      v => v.length >= 3 || "Введите минимум 3 символа"
    ],
    priceRules: [
      v => !!v || "Введите минимум 1 символ",
      v => /^[0-9]+$/.test(v) || "Вводите только цифры"
    ]
  }),
  methods: {
    async addProduct() {
      const product = {
        name: this.name,
        code: this.code,
        price: this.price
      };
      try {
        await fetch("http://grizzly-dev.site/api/test/products/store", {
          method: "POST",
          body: JSON.stringify(product),
          headers: new Headers({
            "Content-Type": "application/json"
          })
        });

        this.$refs.form.reset();
      } catch (e) {
        console.error(e);
      }
    },
    async putProduct() {
      const product = {
        name: this.putName,
        code: this.putCode,
        price: this.putPrice
      };
      try {
        await fetch(
          `http://grizzly-dev.site/api/test/products/${this.putId}/update`,
          {
            method: "PUT",
            body: JSON.stringify(product),
            headers: new Headers({
              "Content-Type": "application/json"
            })
          }
        );
        this.$refs.putForm.reset();
      } catch (e) {
        console.error(e);
      }
    },

    async deleteProduct() {
      try {
        await fetch(
          `http://grizzly-dev.site/api/test/products/${this.id}/destroy`,
          {
            method: "DELETE",
            headers: new Headers({
              "Content-Type": "application/json"
            })
          }
        );
        this.$refs.deleteForm.reset();
      } catch (e) {
        alert(`Продукта с id ${this.putId} нет`);
        console.error(e);
      }
    }
  }
};
</script>

<style>
.form-btn {
  display: flex;
  justify-content: center;
  width: 100%;
}
.mr-4 {
  display: inline-block;
}
.center {
  justify-content: center;
}
</style>
