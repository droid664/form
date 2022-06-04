<template>
  <div>
    <form v-if="!success">
      <label>Имя</label>
      <input
        @input="error = ''"
        v-model="firstName"
        type="text"
        placeholder="Имя"
      />
      <label>Фамилия</label>
      <input
        @input="error = ''"
        v-model="lastName"
        type="text"
        placeholder="Фамилия"
      />
      <button @click.prevent="submit" type="submit">Отправить</button>
    </form>
    <div v-else>{{ resultMessage }}</div>
    <div class="error" v-if="error">{{ error }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      error: '',
      firstName: '',
      lastName: '',
      success: '',
    };
  },
  methods: {
    async submit() {
      if (!this.firstName || !this.lastName) {
        this.error = 'Заполните все поля';
      }

      let message = `
        <h1>Пользователь</h1>
        <p><b>${this.firstName}</b></p>
        <p><b>${this.lastName}</b></p>
      `;

      const { status } = await this.$axios.post(
        'http://localhost:1337/api/email',
        {
          to: 'rodok.test@yandex.ru',
          from: 'iskandarov.murat@gmail.com',
          subject: 'New user',
          html: message,
        }
      );

      if (status == 200) {
        this.success = 'Форма успешно отправлена';
      } else {
        this.success = 'Ошибка :(';
      }
    },
  },
};
</script>

<style>
.error {
  color: red;
}
</style>