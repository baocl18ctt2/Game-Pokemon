<template>
  <div
    class="m-register"
    v-on:blur="handleBlur"
  >
    <div class="m-container">
      <h1>PLAYER FormInformation</h1>
      <div class="icon-close">
        <i class="fas fa-times"></i>
      </div>
      <form @submit="submitForm">
        <!-- Phần tên -->
        <div class="m-row">
          <label form="playName">
            <span class="m-attr-text">Play Name</span>
            <input
              class="m-input"
              :class="{error: error.status}"
              type="text"
              placeholder="Nguyen Van A"
              v-model="register.name"
            />
          </label>
        </div>
        <!-- Phần email -->
        <div class="m-row">
          <label form="Email">
            <span class="m-attr-text">Email Address</span>
            <input
              class="m-input"
              type="email"
              placeholder="example@gmail.com"
              v-model="register.email"
            />
          </label>
        </div>
        <!-- Phần submit -->
        <div class="m-row">
          <button
            class="m-btn m-btn-sign-up"
            type="submit"
          >Start Game</button>
        </div>
      </form>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      isForm1: false,
      register: {
        name: "",
        email: "",
      },
      error: {
        text: "",
        status: false,
      },
    };
  },
  methods: {
    submitForm() {
      // Kiểm tra cú pháp địa chỉ email
      // if (this.register.email != "") {
      //   let re = /^\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}$/;
      //   let validateEmail = re.test(this.register.email);
      //   if (!validateEmail) {
      //     console.log("looix");
      //     this.$emit("formMode", true);
      //   } else {
      //     console.log("fea");

      //   }
      // }
      this.$emit("onStartGame");
    },
    handleBlur(e) {
      console.log("blur", e.target.placeholder);
    },
  },
};
</script>
<style scoped>
@import url("../styles/main.css");
.m-register {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.8);
  width: 100%;
  height: 100vh;
  z-index: 98;
}
.m-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 99;
  width: 600px;
  padding: 50px;
}
.m-attr-text {
  font-size: 20px;
  color: #fff;
}
h1 {
  color: #fff;
  margin-bottom: 30px;
  text-transform: uppercase;
}
.icon-close {
  color: #fff;
  position: absolute;
  top: 0;
  right: 0;
  font-size: 30px;
}
</style>
