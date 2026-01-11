<template>
  <input
    @input="sendInput"
    v-model="userInput"
    :type="type"
    :name="name"
    :value="userInput"
    :placeholder="placeholder"
  />
  <p
    v-if="error"
    class="flex items-center gap-3 rounded-lg bg-red-500/30 p-3 text-red-500 [&_svg]:size-5"
  >
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" color="currentColor" fill="none">
      <path
        d="M13.921 21.75C16.2107 21.75 18.0151 21.7521 19.3478 21.5537C20.6865 21.3543 21.7726 20.9217 22.3722 19.8633C22.9712 18.8058 22.7862 17.6499 22.2726 16.3945C21.7611 15.1444 20.8363 13.5908 19.6651 11.6162L17.7403 8.37109C16.6157 6.47504 15.7304 4.9772 14.9073 3.96484C14.079 2.94615 13.1807 2.25 11.9962 2.25C10.8118 2.25004 9.91333 2.94615 9.08505 3.96484C8.26196 4.97721 7.37666 6.47505 6.25204 8.37109L4.32723 11.6162C3.15604 13.5908 2.23132 15.1444 1.71981 16.3945C1.20621 17.6498 1.02126 18.8058 1.6202 19.8633C2.21978 20.9217 3.30591 21.3543 4.64462 21.5537C5.9773 21.7522 7.78168 21.75 10.0714 21.75H13.921ZM10.0714 20.25C7.73102 20.25 6.06205 20.2483 4.8663 20.0703C3.6768 19.8932 3.17842 19.5716 2.92489 19.124C2.67098 18.6755 2.65137 18.0802 3.10848 16.9629C3.56787 15.8401 4.42019 14.3991 5.61727 12.3809L7.54208 9.13574C8.69235 7.19645 9.51302 5.8165 10.2491 4.91113C10.9798 4.01248 11.494 3.75004 11.9962 3.75C12.4985 3.75 13.0125 4.01245 13.7433 4.91113C14.4794 5.81649 15.3 7.19643 16.4503 9.13574L18.3751 12.3809C19.5722 14.3991 20.4245 15.8401 20.8839 16.9629C21.341 18.0802 21.3214 18.6755 21.0675 19.124C20.8139 19.5715 20.3156 19.8932 19.1261 20.0703C17.9303 20.2483 16.2613 20.25 13.921 20.25H10.0714ZM11.9962 14.25C12.4104 14.25 12.7462 13.9142 12.7462 13.5V9C12.7462 8.58579 12.4104 8.25 11.9962 8.25C11.582 8.25 11.2462 8.58579 11.2462 9V13.5C11.2462 13.9142 11.582 14.25 11.9962 14.25ZM11.9962 17.9004C12.4933 17.9004 12.8966 17.4971 12.8966 17V16.9902C12.8966 16.4932 12.4933 16.0898 11.9962 16.0898C11.4991 16.0898 11.0958 16.4932 11.0958 16.9902V17C11.0958 17.4971 11.4991 17.9004 11.9962 17.9004Z"
        fill="currentColor"
      />
    </svg>
    <span>{{ error }}</span>
  </p>
</template>

<script>
export default {
  props: {
    type: { type: String, required: true },
    name: { type: String },
    placeholder: { type: String },
    value: { type: String },
    rules: { type: Object },
  },
  emits: ['collect'],
  computed: {
    error() {
      return this.validate(this.userInput)
    },
  },
  data() {
    return {
      userInput: this.value,
      hasChanged: false,
    }
  },
  methods: {
    validate() {
      if (this.rules && (this.hasChanged || this.userInput)) {
        if (this.rules.required && !this.userInput) {
          return `${this.name} is required`
        }
        if (this.rules.min && this.rules.min > this.userInput.length) {
          return `${this.name} must be more than ${this.rules.min} characters`
        }
      }
    },
    sendInput() {
      this.hasChanged = true
      this.$emit('collect', {
        is_valid: !this.error && this.hasChanged ? true : false,
        inputName: this.name,
      })
    },
  },
}
</script>

<style scoped></style>
