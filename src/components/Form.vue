<template>
  <form class="form" @submit.prevent="onSubmit">
    <h1 class="">Заполните форму</h1>
    <div class="form__input">
      <label class="">Имя:</label>
      <input type="text" v-model="firstName" />
    </div>
    <div v-if="v$.firstName.$invalid">Name field has an error.</div>

    <div class="form__input">
      <label class="">Фамилия:</label>
      <input type="text" v-model="lastName" />
    </div>

    <div class="form__input">
      <label class="">Email:</label>
      <input type="text" v-model="email" />
    </div>
    <div v-if="v$.email.$error">Name field has an error.</div>

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
      <label v-for="(word, index) in keyWords" :key="word" @click="onDeleteSkill(index)">{{
        word
      }}</label>
    </div>

    <div class="form__textarea">
      <label>Содержание:</label>
      <textarea></textarea>
    </div>

    <div
      class="form__dropzone"
      @dragenter.prevent="toggleActiveZone"
      @dragleave.prevent="toggleActiveZone"
      @dragover.prevent
      @drop.prevent="onDrop"
      :class="{ form__dropzone_active: activeZone }"
    >
      <span>Перенесите сюда файл для загрузки</span>
      <span>или воспользуйтесь кнопкой</span>
      <label for="dropzone">Загрузить</label>
      <input type="file" id="dropzone" accept="image/*, .png, .jpg" @change="onDrop($event)" />
      <span>Файлы формата <b>jpg/png</b>, размером не более <b>2мб</b></span>
    </div>
    <div class="form__dropfiles">
      <div v-if="dropFiles.length">Загруженные файлы:</div>
      <div v-for="(file, index) in dropFiles" :key="file.name">
        <p>
          {{ file.name }}
        </p>
        <span>/ {{ Math.floor(file.size / 1036) }}кб</span>
        <button @click.prevent="onDeleteFile(index)">х</button>
      </div>
    </div>

    <div class="form__checkbox">
      <input type="checkbox" id="terms" v-model="terms" />
      <label for="terms">Соглашаюсь с обработкой персональных данных</label>
    </div>

    <button type="submit" class="form__submit-button">Отправить</button>
  </form>
</template>

<script>
import { useVuelidate } from '@vuelidate/core';
import { required, email, minLength } from '@vuelidate/validators';

export default {
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      firstName: '',
      lastName: '',
      email: '',
      category: 'request',
      terms: 'false',
      tempKeyWords: '',
      keyWords: ['vvv', 'еще чет'],
      dropFiles: [
        { name: '2M6A5273.JPG', lastModified: 1679469630000, size: 5019239 },
        { name: '2M6A5273.JPG', lastModified: 1679469630000, size: 5019239 },
      ],
      activeZone: false,
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
    onDeleteSkill(index) {
      this.keyWords.splice(index, 1);
    },
    toggleActiveZone() {
      this.activeZone = !this.activeZone;
    },
    onDrop(e) {
      const validation = (target) => {
        if (this.dropFiles.some((el) => el.name === target.name) || target.size / 1036 > 2000) {
          return true;
        } else {
          return false;
        }
      };

      if (e.target.files) {
        if (validation(e.target.files[0])) {
          return;
        } else {
          this.dropFiles.push(e.target.files[0]);
          console.log(e.target.files[0]);
        }
      } else if (e.dataTransfer.files) {
        if (validation(e.dataTransfer.files[0])) {
          this.activeZone = !this.activeZone;
          return;
        } else {
          console.log(e.dataTransfer.files);
          this.dropFiles.push(e.dataTransfer.files[0]);
          this.activeZone = !this.activeZone;
        }
      }
      console.log(this.dropFiles);
    },
    onDeleteFile(index) {
      this.dropFiles.splice(index, 1);
    },
    onSubmit() {
      console.log(this.firstName);
    },
  },
  validations() {
    return {
      firstName: { required, minLength: 5 },
      lastName: { required },
      email: { required, email }, // Matches this.contact.email
    };
  },
};
</script>

<style lang="scss" scoped>
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
      margin-bottom: 10px;
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

  &__dropzone {
    width: 40%;
    height: 140px;
    margin-bottom: 20px;
    padding: 10px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    row-gap: 6px;
    border: 2px dashed #41b883;
    background-color: #fff;
    transition: 0.3s ease all;

    span {
      text-align: center;
    }

    label {
      margin-top: 4px;
      padding: 8px 12px;
      color: #fff;
      background-color: #41b883;
      transition: 0.3s ease all;
      cursor: pointer;

      &:hover {
        opacity: 0.7;
      }
    }

    input {
      opacity: 0;
      height: 0;
      width: 0;
      line-height: 0;
      overflow: hidden;
      padding: 0;
      margin: 0;
    }

    &_active {
      width: 45%;
      height: 170px;
      color: #fff;
      border-color: #fff;
      background-color: #41b883;

      label {
        background-color: #fff;
        color: #41b883;
      }
    }
  }

  &__dropfiles {
    display: flex;
    width: 70%;
    flex-direction: column;
    margin-bottom: 10px;

    div {
      display: flex;
      max-width: 65%;
      margin-bottom: 2px;
      border-radius: 10px;
      white-space: nowrap;

      &:first-of-type {
        margin-bottom: 7px;
        background-color: transparent;
      }
    }

    p {
      max-width: 60%;
      margin: 0;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    span {
      margin-right: 5px;
    }

    button {
      margin-top: -3px;
      background-color: transparent;
      border: none;
      cursor: pointer;
    }
  }

  &__checkbox {
    width: 70%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 20px;

    label {
      margin-left: 5px;
      font-size: 0.9rem;
      cursor: pointer;
    }

    input {
      cursor: pointer;
    }
  }
}
</style>
