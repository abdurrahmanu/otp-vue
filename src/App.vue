<template>
  <main>
    <div class="my-10 text-center text-xl">OTP INPUT</div>
    <p class="text-center mb-5">Use CTRL + V to paste from clipboard</p>
    <div class="flex gap-4">
      <input
      v-for="i in length_of_otp"
      :key="i"
      @keypress.space.prevent
      @focus="(current_input_index = i)"
      @keyup="otp_index($event, i - 1)"
      @paste="paste_clipboard_text"
      ref="input_nodes"
      v-model="otp[i - 1]"
      :class="{'border-blue-400 border': i === current_input_index}"
      class="block p-2 w-16 h-16 rounded-md text-center outline-none text-2xl border-black border"
      maxlength="1"
      >
    </div>
    <div class="mt-5 text-center">
      <button
      @click="show_otp"
      class="p-3 bg-blue-400 text-white rounded-2xl px-5 hover:bg-blue-600 active:scale-95">SUBMIT</button>
    </div> 
    <div class="text-center text-2xl mt-10 flex items-center bg-gray-400 p-2 w-fit m-auto rounded-md justify-center gap-2">OTP:
      <span class="border-gray-400 px-5 border inline-block w-fit  rounded-sm h-10 bg-gray-200">
        {{otp_val}}
      </span>
    </div>
  </main>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const input_nodes = ref(null);
const length_of_otp = ref(5)
const otp_val = ref('')
const otp = ref(new Array(length_of_otp.value).fill(''))
const current_input_index = ref(0)

onMounted(() => input_nodes.value[0].focus())

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
  if (input_nodes.value[index + 1]) {
    if (input_nodes.value[index].value.length || e.key === 'ArrowRight') input_nodes.value[index + 1].focus();
  }

  if (input_nodes.value[index - 1]) {
    if (e.key === 'Backspace' || e.key === 'ArrowLeft') input_nodes.value[index - 1].focus()
  }

  if (e.key === 'Enter') show_otp()
}

const show_otp = () => {
  const otp_complete = otp.value.some(each => !each.length )
  otp_complete ? otp_val.value = 'incomplete otp' : otp_val.value = otp.value.join('');
}

</script>


