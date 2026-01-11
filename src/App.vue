<script>
import CInput from './components/CInput.vue'

export default {
  components: { CInput },
  data() {
    return {
      username: [],
      password: [],
    }
  },
  computed: {
    isFormValid() {
      return this.username.is_valid && this.password.is_valid
    },
  },
  methods: {
    getStatus(status) {
      if (status.inputName == 'username') {
        this.username = status
      } else if (status.inputName == 'password') {
        this.password = status
      }
    },
  },
}
</script>

<template>
  <form
    action=""
    @submit="$event.preventDefault()"
    class="absolute top-2/4 left-2/4 w-full max-w-112.5 -translate-2/4"
  >
    <img
      src="./../public/images/tailwind.svg"
      alt="Your Company"
      class="mx-auto h-10 w-auto select-none"
    />
    <h1 class="mt-3 text-center text-2xl font-medium">Welcome back!</h1>
    <p class="mt-2 text-center text-slate-50/60">Enter your credentials to access your account</p>
    <div class="mt-8 grid gap-3">
      <c-input
        @collect="getStatus"
        name="username"
        type="text"
        placeholder="Username"
        value="da1"
        :rules="{ required: true, min: 4 }"
      />
      <c-input
        @collect="getStatus"
        name="password"
        type="password"
        placeholder="Password"
        :rules="{ required: true, min: 8 }"
      />
    </div>
    <button :disabled="!isFormValid" class="mt-4 w-full">Login</button>
  </form>
</template>

<style scoped></style>
