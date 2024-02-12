<template>
  <main class="h-screen m-auto">
    <div class="flex gap-4">
      <input
      v-for="i in length_of_otp"
      :key="i"
      @focus="current_input_index = i"
      @keypress.space.prevent
      @keyup="otp_index($event, i - 1)"
      @paste="paste_clipboard_text"
      ref="input_nodes"
      v-model="otp[i - 1]"
      :class="{'border-2 border-gray-300 ': current_input_index == i}"
      class="block p-2 w-16 shadow-md shadow-gray-300 h-16 rounded-md text-center outline-none text-2xl border-black border"
      maxlength="1">
    </div>
    <div class="text-center py-3">
      <button
      @click="show_otp"
      class="p-3 bg-blue-300 rounded-2xl px-5">SUBMIT</button>
    </div>
    <div class="text-center py-3">
        "payload": {
        "otp": "{{otp_val}}"
        }
    </div>
  </main>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const input_nodes = ref(null);
const length_of_otp = ref(5);
const otp_val = ref('')
const otp = ref([])
otp.value.length = length_of_otp.value
otp.value.fill('')
const current_input_index = ref(0)

const paste_clipboard_text =  async () => {
  const clipboard_text = await navigator.clipboard.readText()

  input_nodes.value.map((each, i) => {
    if (clipboard_text[i] === ' ') {
      each.value = '';
      otp.value[i] = ''
    }
    else {
      each.value = clipboard_text[i]
      otp.value[i] = clipboard_text[i]
    }
    })
  input_nodes.value[input_nodes.value.length - 1].focus()
}

const otp_index = (e, index) => {  
  if (e.key === 'Enter') show_otp()

  if (input_nodes.value[index + 1]) {
    if (input_nodes.value[index].value.length || e.key === 'ArrowRight') input_nodes.value[index + 1].focus();
  }

  if (input_nodes.value[index - 1]) {
    if (e.key === 'Backspace' || e.key === 'ArrowLeft') input_nodes.value[index - 1].focus()
  }

  //maxlength for last otp-input mobile fallback
  if (index === input_nodes.value.length - 1) {
    if (input_nodes.value[index].value) input_nodes.value[index].value = input_nodes.value[index].value[0]
  }
}

function show_otp() {
const otp_complete = otp.value.some(each => !each.length);
otp_complete ? otp_val.value = 'incomplete otp' : otp_val.value = otp.value.join('');
}
</script>












