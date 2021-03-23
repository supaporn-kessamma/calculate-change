<template>
  <div>
    <v-modal
      class="lt-modal relative overflow-visible"
      :name="name"
      v-bind="modalProps"
    >
      <client-only v-if="showClose">
        <div v-if="showHeader" class="header">
          <span class="under-header">{{ nameHeader }}</span>
        </div>
        <span class="close" @click="$emit('close')">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="h-8 w-8 text-red-600"
          >
            <path
              fill-rule="evenodd"
              d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
              clip-rule="evenodd"
            />
          </svg>
        </span>
        <slot></slot>
        <div v-if="showButton" class="button">
          <span class="under-button" @click="$emit('apply')">{{
            nameButton
          }}</span>
        </div>
      </client-only>
    </v-modal>
  </div>
</template>

<script>
export default {
  props: {
    showClose: {
      type: Boolean,
      default: true,
    },
    show: {
      type: Boolean,
      default: false,
    },
    showHeader: {
      type: Boolean,
      default: true,
    },
    showButton: {
      type: Boolean,
      default: true,
    },
    nameHeader: {
      type: String,
      default: '',
    },
    nameButton: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      name: '',
    }
  },
  computed: {
    modalProps() {
      return {
        adaptive: true,
        clickToClose: false,
        height: 'auto',
      }
    },
  },
  watch: {
    show: {
      handler(val) {
        if (val) {
          this.$modal.show(this.name)
        } else {
          this.$modal.hide(this.name)
        }
      },
    },
  },
}
</script>
<style>
.lt-modal .vm--modal {
  @apply overflow-visible;
  background: #ffffff;
  @apply rounded-md;
}
</style>
<style scoped>
.close {
  @apply absolute;
  top: 2px;
  right: 4px;
  @apply w-12;
  @apply h-12;
  @apply flex;
  @apply justify-center;
  @apply items-center;
  @apply cursor-pointer;
}
.header {
  @apply flex;
  @apply justify-center;
  @apply items-center;
  @apply text-2xl;
  @apply -mt-5;
}
.under-header {
  @apply inline-block;
  @apply px-16;
  @apply py-2;
  @apply rounded-xl;
}
.button {
  @apply flex;
  @apply justify-center;
  @apply items-center;
  @apply text-3xl;
  @apply mt-16;
}
.under-button {
  @apply absolute;
  @apply bg-green-600;
  @apply text-white;
  bottom: -30px;
  @apply px-8;
  @apply py-2;
  @apply rounded-xl;
  @apply cursor-pointer;
}
</style>
