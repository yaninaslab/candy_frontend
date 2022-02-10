<template>
  <div>
    <p>{{ error_message }}</p>
    <h1 class="title">Valentine's Chocolate and Candy</h1>
    <button @click="show_modal" class="add_btn">New Candy</button>
    <div v-if="is_modal" class="modal active">
      <div class="modal_header">
        <button @click="close_modal" class="close-button">&times;</button>
        <input
          class="candy"
          ref="name_input"
          placeholder="Your candy name"
          maxlength="100"
        />
        <input
          class="candy"
          ref="desc_input"
          placeholder="Description"
          maxlength="100"
        />
        <input
          class="candy"
          ref="url_input"
          placeholder="Image_URL"
          maxlength="500"
        />

        <br />
      </div>
      <input
        class="candy_btn"
        @click="add_candy"
        type="submit"
        ref="add_candy"
        value="Add Candy"
      />
    </div>
    <article class="candy_container">
      <div class="item_border" v-for="candy in candies" :key="candy[0]">
        <img :src="candy[3]" :alt="candy[1]" />
        <h3>{{ candy[1] }}</h3>
        <p class="desc">{{ candy[2] }}</p>
        <div class="cart">
          <button @click="update_candy(candy[0])" class="cart_btn">+</button>
          <div>{{ candy[4] }}</div>
          <button @click="delete_candy(candy[0])" class="cart_btn">-</button>
        </div>
      </div>
    </article>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "candy-list",
  data() {
    return {
      candies: [],
      error_message: "",
      is_modal: false,
    };
  },
  mounted() {
    axios
      .request({
        url: "http://127.0.0.1:5000/candy",
      })
      .then((res) => {
        console.log(res);
        this.candies = res.data;
      })
      .catch((err) => {
        console.log(err);
        this.error_message = "Sorry, something went wrong!";
      });
  },
  methods: {
    update_candy(candy_id) {
      axios
        .request({
          url: "http://127.0.0.1:5000/candy",
          method: "PATCH",
          data: {
            candy_id: candy_id,
          },
        })
        .then(() => {
          this.change_candy();
        })
        .catch((error) => {
          error.message;
        });
    },

    delete_candy(candy_id) {
      axios
        .request({
          url: "http://127.0.0.1:5000/candy",
          method: "DELETE",
          data: {
            candy_id: candy_id,
          },
        })
        .then(() => {
          this.remove_candy();
        })
        .catch((error) => {
          error.message;
        });
    },

    add_candy() {
      axios
        .request({
          url: "http://127.0.0.1:5000/candy",
          method: "POST",
          data: {
            name: this.$refs.name_input.value,
            description: this.$refs.desc_input.value,
            image_url: this.$refs.url_input.value,
          },
        })
        .then((response) => {
          this.candies.push(response.data);
          ((this.$refs.name_input.value = ""),
          (this.$refs.desc_input.value = ""),
          (this.$refs.url_input.value = "")),
            this.$emit("candy_added", "Candy has been successfully added!");
        })
        .catch((error) => {
          error.message;
        });
    },

    change_candy() {
      this.candies = Response.data.length;
      for (var i = 0; i < this.candies; i++) {
        if (this.candy_id == Response.data.id) {
          this.candy[4] = this.candy[4] + 1;
        }
      }
    },
    remove_candy() {
      this.candies = Response.data.length;
      for (var i = 0; i < this.candies; i++) {
        if (this.candy_id == Response.data.id) {
          this.candy[4] = this.candy[4] - 1;
        }
      }
    },
    show_modal() {
      this.is_modal = true;
    },
    close_modal() {
      this.is_modal = false;
    },
  },
};
</script>

<style scoped>
.candy_container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 30px;
  margin-top: 30px;
}
.item_border {
  border: 1px solid gold;
  border-radius: 15%;
  background: lightgoldenrodyellow;
  padding-bottom: 10px;
}
img {
  margin-top: 15px;
  max-width: 120px;
}
h1 {
  color: red;
  text-shadow: -1px 0 goldenrod, 0 1px goldenrod, 1px 0 goldenrod,
    0 -1px goldenrod;
}
h3,
.desc {
  color: brown;
}
.cart {
  display: inline-flex;
}
button {
  margin-left: 10px;
  margin-right: 10px;
}
.cart_btn {
  width: 35px;
  height: 30px;
  background-color: burlywood;
  border: burlywood;
  border-radius: 15%;
  color: brown;
  font-size: 1.5rem;
  font-weight: bolder;
}
.add_btn {
  width: 150px;
  height: 70px;
  background-color: burlywood;
  border: burlywood;
  border-radius: 15px;
  color: brown;
  font-size: 1.5rem;
  font-weight: bolder;
}
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0);
  transition: 200ms ease-in-out;
  border: 1px solid gold;
  border-radius: 5px;
  z-index: 10;
  background-color: rgb(251, 250, 238);
  width: 100%;
  min-height: 300px;
  max-width: 80%;
  border-radius: 10px;
}
.modal.active {
  transform: translate(-50%, -50%) scale(1);
}
.candy {
  width: 200px;
  margin-top: 70px;
  margin-right: 10px;
  border-radius: 5px;
  background: white;
  border: 1px solid burlywood;
  outline: none;
  padding: 10px;
}
.candy_btn {
  margin-top: 20px;
  margin-left: 50%;
  width: 150px;
  height: 40px;
  background-color: burlywood;
  border-radius: 5px;
  color: brown;
  font-weight: bold;
  font-size: large;
  border: 1px solid burlywood;
}
.modal_header {
  padding: 10px 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.close-button {
  margin-bottom: 70px;
  cursor: pointer;
  border: none;
  outline: none;
  background: none;
  font-size: 1.5rem;
  font-weight: bold;
  color: burlywood;
}
.overlay {
  position: fixed;
  opacity: 0;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: lightgrey;
  pointer-events: none;
  transition: 200ms ease-in-out;
}
.overlay.active {
  opacity: 1;
  pointer-events: all;
}
</style>
