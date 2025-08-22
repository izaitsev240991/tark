<script setup>
import { ref, watch } from 'vue'

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
  'ru'
])

const onPhoneInput = (e) => {
  form.value.phone = (e?.target?.value ?? '').replace(/\D/g, '')
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
    <div>
      <a href="https://turkov.ru/" target="_blank"><img src="./img/logo.svg" alt="logo" class="footer__logo"></a>
    </div>


    <form
      action=""
      :class="{ 'footer__form-loader': showLoader }"
      class="footer__form"
      @submit.prevent="submitForm"
    >

      <svg v-if="showLoader" class="footer__form-loader__icon" width="60" height="60" viewBox="0 0 60 60" fill="none" xmlns="http://www.w3.org/2000/svg">
        <rect x="27.9305" width="4.13793" height="12.4138" rx="2" fill="#7990B2"/>
        <rect x="27.9305" y="47.5859" width="4.13793" height="12.4138" rx="2" fill="#7990B2"/>
        <rect x="59.9995" y="27.9307" width="4.13793" height="12.4138" rx="2" transform="rotate(90 59.9995 27.9307)" fill="#024DC4"/>
        <rect x="12.4133" y="27.9307" width="4.13793" height="12.4138" rx="2" transform="rotate(90 12.4133 27.9307)" fill="#7990B2"/>
        <rect x="57.0132" y="43.21" width="4.13793" height="12.4138" rx="2" transform="rotate(120 57.0132 43.21)" fill="#7990B2"/>
        <rect x="15.8036" y="19.417" width="4.13793" height="12.4138" rx="2" transform="rotate(120 15.8036 19.417)" fill="#7990B2"/>
        <rect x="46.7896" y="54.9492" width="4.13793" height="12.4138" rx="2" transform="rotate(150 46.7896 54.9492)" fill="#7990B2"/>
        <rect x="22.9965" y="13.7354" width="4.13793" height="12.4138" rx="2" transform="rotate(150 22.9965 13.7354)" fill="#7990B2"/>
        <rect x="16.7896" y="57.0176" width="4.13793" height="12.4138" rx="2" transform="rotate(-150 16.7896 57.0176)" fill="#7990B2"/>
        <rect x="40.5847" y="15.8037" width="4.13793" height="12.4138" rx="2" transform="rotate(-150 40.5847 15.8037)" fill="#7990B2"/>
        <rect x="5.05066" y="46.7939" width="4.13793" height="12.4138" rx="2" transform="rotate(-120 5.05066 46.7939)" fill="#7990B2"/>
        <rect x="46.2643" y="23.001" width="4.13793" height="12.4138" rx="2" transform="rotate(-120 46.2643 23.001)" fill="#7990B2"/>
      </svg>

      <div v-if="showThx" class="footer__form-thx">
        <img src="./img/checkmark.svg" alt="">
        <div class="footer__form-title">Спасибо!</div>
        <div class="footer__form-subtitle">
          Ваша заявка принята. <br>
          Наш инженер свяжется с вами в ближайшее время
        </div>
        <button type="button" @click="writeAgain">Написать ещё</button>
      </div>

      <div v-if="!showThx">
        <div class="footer__form-title">У вас остались вопросы?</div>
        <div class="footer__form-subtitle">Напишите их нам, мы поможем во всём разобраться</div>
      </div>

      <div v-if="!showThx" class="footer__forms">
        <label class="form__label">
          <input
            type="text"
            @focus="activeField = 'name'"
            @blur="onBlurHandler('name')"
            v-model="form.name"
            class="form__input"
            :class="{ 'error': errors.name }"
            placeholder=" "
          />
          <span :class="{ hidden: form.name && activeField !== 'name' }">Имя *</span>
          <p v-if="errors.name" class="error-message">Поле обязательно для заполнения</p>
        </label>
        <label class="form__label">
          <input
            type="tel"
            @focus="activeField = 'phone'"
            @blur="onBlurHandler('phone')"
            @input="onPhoneInput"
            v-model="form.phone"
            class="form__input"
            placeholder=" "
          />
          <span :class="{ hidden: form.phone && activeField !== 'phone' }">Телефон</span>
        </label>
        <label class="form__label">
          <input
            type="email"
            @focus="activeField = 'email'"
            @blur="onBlurHandler('email')"
            v-model="form.email"
            class="form__input"
            :class="{ 'error': errors.email }"
            placeholder=" "
          />
          <span :class="{ hidden: form.email && activeField !== 'email' }">Email *</span>
          <p v-if="errors.email" class="error-message">Поле обязательно для заполнения</p>
        </label>
        <label class="form__label">
          <textarea
            id="message"
            v-model="form.message"
            class="form__input"
            @focus="activeField = 'message'"
            @blur="onBlurHandler('message')"
            :class="{ 'error': errors.message }"
            placeholder=" "
            rows="4"
          ></textarea>
          <span :class="{ hidden: form.message && activeField !== 'message' }">Текст письма *</span>
          <p v-if="errors.message" class="error-message">Поле обязательно для заполнения</p>
        </label>
        <label class="footer__agree">
          <input
            type="checkbox"
            v-model="form.agree"
            :class="{ 'error': errors.agree }"
          />
          <div class="footer__agree-check"></div>
          <div>
            <span>Даю согласие</span>
            <a href="https://turkov.ru/company/processing-data/" target="_blank">на обработку своих персональных данных</a>
          </div>
          <p v-if="errors.agree" class="error-message">Необходимо ваше согласие</p>
        </label>

        <button class="footer__form-button" type="submit">Отправить</button>
      </div>
    </form>

    <div class="footer__socials">
      <a target="_blank" v-for="(href, i) in socials" :key="i" :href="href"></a>
    </div>
    <div class="footer__info">
      <div class="footer__info-links">
        <a href="https://turkov.ru/company/cookie-notice/" target="_blank">Уведомление о файле cookie</a>
        <a href="https://turkov.ru/company/processing-data/" target="_blank">Политика обработки персональных данных</a>
      </div>
      <div class="footer__info-copyright">© 2009-{{ currentYear }} Все права защищены</div>
    </div>
  </footer>
</template>

<style>
@import './footer.scss'
</style>
git config user.name "Ilya Zaytsev"
git config user.email "ВАШ_EMAIL@пример.com"
