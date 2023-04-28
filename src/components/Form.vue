<template>
  <form class="form" @submit.prevent="onSubmit">
    <h1 class="">Заполните форму</h1>
    <div class="form__input">
      <label class="">Имя:</label>
      <input type="text" required v-model="firstName" />
    </div>

    <div class="form__input">
      <label class="">Фамилия:</label>
      <input type="text" required v-model="secondName" />
    </div>

    <div class="form__input">
      <label class="">Email:</label>
      <input type="email" required v-model="email" />
    </div>

    <div class="form__input">
      <label>Категория:</label>
      <select v-model="category">
        <option value="request">Обращение</option>
        <option value="employment">Трудоустройство</option>
        <option value="feedback">Отзыв</option>
      </select>
    </div>

    <div class="form__input">
      <label>Ключевые слова:</label>
      <input type="text" id="keyWords" v-model="tempKeyWords" @keyup="addKeyWord" />
    </div>
    <label for="keyWords" class="form__advice-label">
      Введите сюда ключевые слова через запятую, это поможет быстрее отработать ваш запрос
    </label>
    <div class="form__skills">
      <label v-for="(word, index) in keyWords" :key="word" @click="onDelete(index)">{{
        word
      }}</label>
    </div>

    <div class="form__textarea">
      <label>Содержание:</label>
      <textarea></textarea>
    </div>

    <div class="form__checkbox">
      <input type="checkbox" id="terms" v-model="terms" />
      <label for="terms">Соглашаюсь с обработкой персональных данных</label>
    </div>

    <button type="submit">Отправить</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      firstName: '',
      secondName: '',
      email: '',
      category: 'request',
      terms: 'false',
      tempKeyWords: '',
      keyWords: ['vvv', 'еще чет'],
    };
  },
  methods: {
    addKeyWord(e) {
      if (e.key === ',' && this.tempKeyWords.length > 0) {
        if (!this.keyWords.includes(this.tempKeyWords.substring(0, this.tempKeyWords.length - 1))) {
          this.keyWords.push(this.tempKeyWords.substring(0, this.tempKeyWords.length - 1));
        }
        this.tempKeyWords = '';
      }
    },
    onDelete(index) {
      this.keyWords.splice(index, 1);
    },
    onSubmit() {
      console.log(this.firstName);
    },
  },
};
</script>

<style lang="scss">
.form {
  margin: 40px 0 30px;
  padding: 60px 20px;
  width: 50%;
  border-radius: 20px;
  background-color: white;
  display: flex;
  flex-direction: column;
  align-items: center;

  h1 {
    margin: 0 0 40px;
    font-size: 1.8rem;
    text-align: center;
  }

  &__input {
    margin: 0 auto;
    white-space: nowrap;
    width: 70%;
    display: flex;
    justify-content: space-between;

    label {
      font-size: 1.1rem;
    }

    input {
      width: 70%;
      margin-left: 5px;
      margin-bottom: 15px;
      padding: 0 0 3px;
      border: none;
      border-bottom: 1px solid rgba(0, 0, 0, 0.363);
      outline: none;
      text-align: center;
      font-size: 1.1rem;
      color: #000000b8;
    }

    select {
      margin-bottom: 15px;
    }
  }

  &__checkbox {
    width: 70%;
    display: flex;
    align-items: center;

    label {
      margin-left: 5px;
      font-size: 0.9rem;
      cursor: pointer;
    }

    input {
      cursor: pointer;
    }
  }

  &__skills {
    width: 70%;
    margin-bottom: 10px;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;

    label {
      margin-right: 5px;
      margin-bottom: 5px;
      padding: 6px;
      border-radius: 5px;
      background-color: #ececec;
      cursor: pointer;
    }
  }

  &__advice-label {
    font-size: 0.7rem;
    margin-top: -17px;
    width: 70%;
    text-align: center;
    visibility: hidden;
    transition: ease-in-out 0.5s;
  }

  &__textarea {
    width: 70%;
    margin-bottom: 10px;
    display: flex;
    flex-direction: column;

    label {
      margin-bottom: 5px;
      font-size: 1.1rem;
    }

    textarea {
      max-width: 100%;
      border: 1px solid rgba(0, 0, 0, 0.363);
      border-radius: 4px;
      padding: 4px;
      outline: none;
      height: 55px;
      transition: ease-in-out 0.2s;
      font-size: 1rem;
      font-style: italic;
      resize: none;

      &:focus {
        height: 120px;
      }
    }
  }
}
</style>
