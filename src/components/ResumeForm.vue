<template>
  <form class="card card-w30" @submit.prevent="submit">
    <div class="form-control">
      <label for="type">Type</label>
      <select id="type" v-model="type">
        <option value="title">Title</option>
        <option value="subtitle">Subtitle</option>
        <option value="avatar">Avatar</option>
        <option value="text">Text</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Value</label>
      <textarea id="value" rows="3" v-model="value"></textarea>
    </div>

    <button class="btn primary" :disabled="!isValid">Add</button>
  </form>
</template>

<script>
  export default {
    emits: {
      ['block-added'](value) {
        if (typeof value !== 'object' || value === null) {
          return false;
        }

        const isHasIdProperty = value.hasOwnProperty('id');
        const isHasTypeProperty = value.hasOwnProperty('type');
        const isHasValueProperty = value.hasOwnProperty('value');
        const isIdNumberType = typeof value.id === 'number';
        const isTypeStringType = typeof value.type === 'string';
        const isValueStringType = typeof value.value === 'string';

        return isHasIdProperty && isHasTypeProperty && isHasValueProperty && isIdNumberType && isTypeStringType && isValueStringType;
      }
    },

    data() {
      return {
        type: 'title',
        value: ''
      };
    },

    computed: {
      isValid() {
        return this.value.length > 3;
      }
    },

    methods: {
      submit() {
        this.$emit('block-added', {
          type: this.type,
          value: this.value,
          id: Date.now()
        });

        this.value = '';
        this.type = 'title';
      }
    }
  }
</script>

<style scoped>
</style>