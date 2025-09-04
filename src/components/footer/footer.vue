<script setup>
import { ref, watch } from 'vue'
import IMask from 'imask'

const currentYear = ref(new Date().getFullYear())

const form = ref({
  name: '',
  phone: '',
  email: '',
  message: '',
  agree: false
})

const errors = ref({
  name: false,
  email: false,
  message: false,
  agree: false
})

const activeField = ref(null);
const showLoader = ref(false);
const showThx = ref(false);

const socials = ref([
  'https://t.me/turkov_official',
  'https://vk.com/turkov_havc',
  'https://www.youtube.com/@TURKOV',
  'https://vc.ru/id1547915',
  'https://ok.ru/turkov',
  'https://dzen.ru/turkov?utm_referrer=turkov.ru',
  'https://rutube.ru/channel/24967539/'
])

const vImask = {
  mounted(el, binding) {
    const mask = IMask(el, { mask: '+{7} (000) 000-00-00' })
    el._imask = mask


    mask.on('accept', () => {
      const formatted = mask.value
      if (binding.instance?.form) {
        binding.instance.form.phone = formatted
      }
    })


    if (typeof binding.value === 'string' && binding.value !== mask.value) {
      mask.value = binding.value
    }
  },
  updated(el, binding) {
    const mask = el._imask
    if (mask && binding.value !== mask.value) {
      mask.value = binding.value ?? ''
    }
  },
  beforeUnmount(el) {
    el._imask?.destroy()
    el._imask = undefined
  }
}


const triedSubmit = ref(false)

const isEmailValid = (val) => /\S+@\S+\.\S+/.test(val)
const validateAll = () => {
  errors.value.name = !form.value.name.trim()
  errors.value.email = !form.value.email.trim() || !isEmailValid(form.value.email)
  errors.value.message = !form.value.message.trim()
  errors.value.agree = !form.value.agree
}

watch(() => form.value.name, () => {
  if (!triedSubmit.value) return
  errors.value.name = !form.value.name.trim()
})
watch(() => form.value.email, () => {
  if (!triedSubmit.value) return
  const v = form.value.email
  errors.value.email = !v.trim() || !isEmailValid(v)
})
watch(() => form.value.message, () => {
  if (!triedSubmit.value) return
  errors.value.message = !form.value.message.trim()
})
watch(() => form.value.agree, () => {
  if (!triedSubmit.value) return
  errors.value.agree = !form.value.agree
})


const onBlurHandler = (field) => {
  if (activeField.value === field) {
    activeField.value = null;
  }
};

const submitForm = () => {
  triedSubmit.value = true
  validateAll()

  if (errors.value.name || errors.value.email || errors.value.message || errors.value.agree) {
    return
  }


  showThx.value = false
  showLoader.value = true

  form.value = { name: '', phone: '', email: '', message: '', agree: false }
  errors.value = { name: false, email: false, message: false, agree: false }
  triedSubmit.value = false


  setTimeout(() => {
    showLoader.value = false
    showThx.value = true
  }, 500)
}


const writeAgain = () => {
  showThx.value = false
  showLoader.value = true
  setTimeout(() => {
    showLoader.value = false
  }, 500)
}
</script>

<template>
  <footer class="footer">
    <div class="footer__img">

    </div>






  </footer>
</template>

<style>
@import './footer.scss'
</style>
git config user.name "Ilya Zaytsev"
git config user.email "ВАШ_EMAIL@пример.com"
