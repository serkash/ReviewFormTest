<script setup>
import { ref, watch } from 'vue'
import Alert from './parts/Alert.vue';

const inProcess = ref(false)
const isReviewShow = ref(false)
const title = ref("Пожалуйста, оцените удобство работы с приложением")
const email = ref("Введите электронный адрес")
const comment = ref("Поделитесь мыслями, что можно было бы улучшить (необязательно)")
const send = ref("Отправить отзыв")
const success = ref("Ваш отзыв успешно отправлен!")
const failure = ref("Не удалось отправить отзыв. Повторите попытку.")
const emailValue = ref(null)
const ratingValue = ref("5")
const commentValue = ref(null)
const status = ref(null)

function sendRating() {
  const getData = () => {
    fetch('/api/test', {    
      method: 'GET',
      mode: 'cors',
      cache: 'no-cache',
      credentials: 'same-origin',
      headers: { 'Content-type': 'application/json' },
    }).then(res=>res.json()).then((response) => {
      status.value = response.res
      inProcess.value = true
      setTimeout(() => {
        status.value = null
        inProcess.value = false
      }, 3000)

      if (status.value === "success") {
        emailValue.value = null
        commentValue.value = null
        ratingValue.value = "5"
      }
    }).catch((error) => {
      console.log('Найдена ошибка \n', error);
    });
  }

  getData()
}

watch(ratingValue, async (newRatingValue) => {
  if (parseInt(newRatingValue) < 5) {
    isReviewShow.value = true
  } else {
    isReviewShow.value = false
  }
})
</script>

<template>
    <div class="review-form">
      <div class="review-form__wrapper">
        <form class="review-form__form" @submit.prevent="sendRating">
          <h2 class="review-form__title">{{ title }}</h2>
          <div class="review-form__input review-form__input-rating">
            <input name="rating" id="rating1" type="radio" value="1" v-model="ratingValue">
            <label for="rating1"></label>
            <input name="rating" id="rating2" type="radio" value="2" v-model="ratingValue">
            <label for="rating2"></label>
            <input name="rating" id="rating3" type="radio" value="3" v-model="ratingValue">
            <label for="rating3"></label>
            <input name="rating" id="rating4" type="radio" value="4" v-model="ratingValue">
            <label for="rating4"></label>
            <input name="rating" id="rating5" type="radio" value="5" v-model="ratingValue">
            <label for="rating5"></label>
          </div>
          <div class="review-form__input review-form__input-text">
            <input name="email" type="text" :placeholder="email" v-model="emailValue">
          </div>
          <div class="review-form__input review-form__input-text" v-if="isReviewShow">
            <textarea type="text" :placeholder="comment" rows="10" v-model="commentValue"></textarea>
          </div>
          <div class="review-form__input review-form__input-button">
            <input type="submit" :value="send" :class="{ 'disabled' : inProcess }">
          </div>
        </form>
      </div>

      <Alert :status="status" :status-text="success" @click="status.value = null" v-if="status === 'success'" />
      <Alert :status="status" :status-text="failure" @click="status.value = null" v-if="status === 'failure'" />

    </div>
</template>

<style scoped lang="scss">
  @mixin inputDesign {
    width: 100%;
    border: 0;
    padding: 10px 15px;
    border-radius: 5px;
  }

  .review-form {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;

    &__title {
      text-align: center;
      font-size: 21px;
      font-weight: 700;
      margin-bottom: 20px;
    }

    &__wrapper {
      max-width: 600px;
    }

    &__form {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    &__input {
      width: 100%;
      margin-bottom: 20px;

      &-text {
        & input,
        & textarea {
          @include inputDesign;

          background-color: #ffffff;
        }
      }

      &-rating {
        display: flex;
        align-items: center;
        justify-content: center;

        &:not(:hover) input:indeterminate + label::before,
        &:not(:hover) input:checked ~ input + label::before,
        & input:hover ~ input + label::before {
          content: '\002606'
        }

        & input {
          position: absolute;
          left: -100vw;
        }
        
        & label {
          cursor: pointer;
          color: #fc8507;

          &::before {
            content: '\002605';
            font-size: 2.5rem;
          }
        }
      }

      &-button {
        & input {
          @include inputDesign;

          width: 100%;
          background-color: green;
          transition: all 0.25s ease-out;
          font-weight: 700;
          color: #ffffff;
          cursor: pointer;

          &:hover,
          &:focus {
            opacity: 0.7;
            transition: all 0.25s ease-out;
          }
          &.disabled {
            pointer-events: none;
            background-color: grey;
          }
        }
      }
    }
  }
</style>
