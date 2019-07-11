<template>
  <div class="container">
    <h1>
      Price calculator
    </h1>

    <div class="container">
      <div class="col-md-12">
        <div class="col-md-12">
          <input
            v-model="name"
            type="text"
            class="form-control"
            placeholder="Product Name"
          />
        </div>

        <div class="col-md-12">
          <div class="form-row">
            <div class="col-7">
              <input
                class="form-control"
                type="text"
                v-model="price"
                placeholder="Price"
              />
            </div>
            <p class="text-center">X</p>
            <div class="col">
              <input
                class="form-control"
                v-model="units"
                type="number"
                placeholder="Units"
              />
            </div>
          </div>
        </div>

        <div
          class="col-md-12"
          v-for="discount in discounts"
          v-bind:key="discount.name"
        >
          <discount
            :label="discount.name + ' Discount (%)'"
            :value="discount.value"
            @update="discount.value = arguments[0]"
          />
        </div>

        <div class="row border rounded mx-auto">
          <div class="collapsible" v-on:click="toggleDiscountMaker()">
            <div class="row">
              <div class="col-md-8">
                <p><Strong>Add Discount</Strong></p>
              </div>
              <div class="col-md-4">
                <p class="text-right"><Strong>+</Strong></p>
              </div>
            </div>
          </div>
          <div class="container-fluid" style="margin:30px 10px" v-if="isOpen">
            <div>
              <div class="form-group">
                <label>Name:</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="newDiscountName"
                />
                <p></p>
              </div>
              <div class="form-group">
                <label>Discount value (%):</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="newDiscountValue"
                />
              </div>
              <button class="btn btn-info" v-on:click="addNewDiscount()">
                Add Discount
              </button>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="alert alert-success">
            Total Price: <strong>{{ price * units }}</strong>
          </div>
          <div
            class="alert alert-info"
            v-for="discount in discounts"
            v-bind:key="discount.name"
          >
            {{ discount.name }} price:
            <strong>{{ calculateDiscount(discount.value) }}</strong>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import discount from '~/components/discount.vue'

export default {
  components: {
    discount
  },
  data: function() {
    return {
      name: '',
      price: '',
      units: 1,
      discounts: [],
      newDiscountName: '',
      newDiscountValue: 0,
      isOpen: false
    }
  },
  computed: {},
  methods: {
    toggleDiscountMaker() {
      this.isOpen = !this.isOpen
    },
    addNewDiscount() {
      this.discounts.push({
        name: this.newDiscountName,
        value: this.newDiscountValue
      })
      this.newDiscountName = ''
      this.newDiscountValue = 0
      this.isOpen = false
    },
    calculateDiscount(discountValue) {
      const discount = (100 - discountValue) / 100
      return this.price * this.units * discount
    }
  }
}
</script>

<style>
h1 {
  padding-top: 2.5em;
  color: #41b883;
}

.alert.alert-info {
  background-color: #41b883;
  color: white;
  font-size: 1.2em;
}

.btn.btn-info {
  background-color: #41b883;
  color: white;
  font-size: 1.2em;
}
/* Style the button that is used to open and close the collapsible content */
.collapsible {
  background-color: #eee;
  color: #444;
  cursor: pointer;
  padding: 18px;
  width: 100%;
  border: none;
  text-align: left;
  outline: none;
  font-size: 15px;
}

/* Add a background color to the button if it is clicked on (add the .active class with JS), and when you move the mouse over it (hover) */
.active,
.collapsible:hover {
  background-color: #ccc;
}

/* Style the collapsible content. Note: hidden by default */
.content {
  transition: max-height 0.2s ease-out;
}
</style>
