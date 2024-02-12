<template>
  <main class="py-20">
    <div class="flex gap-4 py-10">
      <input
      v-for="i in length"
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
      <button
      @click="show_otp"
      class="p-1 inline-block m-auto bg-green-300 rounded-2xl selection:bg-transparent px-5">SUBMIT</button>
    <div class="text-center py-3">
        "payload": {
        "otp": "{{otp_val}}",
        "success": "{{success}}"
        }
    </div>
  </main>
</template>

<script setup>
import { onMounted, ref, defineProps} from 'vue';

const input_nodes = ref(null);
const otp_val = ref('')
const success = ref('')
const otp = ref([])
const current_input_index = ref(0)

const props = defineProps({
   length: {
     type: number,
     default: 4,
   }
)}

otp.value.length = props.length
otp.value.fill('')


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

  //maxlength for last otp-input mobile fallback
  if (index === input_nodes.value.length - 1) {
    if (input_nodes.value[index].value) input_nodes.value[index].value = input_nodes.value[index].value[0]
  }
}

function show_otp() {
const otp_incomplete = otp.value.some(each => !each.length);

 if (otp_incomplete) {
    otp_val.value = 'null'
    success.value = 'unsuccesful'   
 } else {
    otp_val.value = otp.value.join('')
    success.value = 'successful'
 }
}
</script>












