<template>
  <div class="card" v-if="comments.length">
    <h2>Comments</h2>
    <ul class="list">
      <li
          class="list-item"
          v-for="comment in comments"
          :key="comment.id"
      >
        <div>
          <p>
            <strong>{{ comment.email }}</strong>
          </p>

          <small>{{ comment.body }}</small>
        </div>
      </li>
    </ul>
  </div>

  <p v-else>
    <button class="btn primary" @click="$emit('load-comments')">Load comments</button>
  </p>
</template>

<script>
  export default {
    emits: {
      ['load-comments'](value) {
        return value === undefined;
      }
    },

    props: {
      comments: {
        type: Array,
        required: true,
        validator(value) {
          return value.every(item => {
            if (typeof item !== 'object' || item === null) {
              return false;
            }

            const isHasIdProperty = item.hasOwnProperty('id');
            const isHasBodyProperty = item.hasOwnProperty('body');
            const isHasEmailProperty = item.hasOwnProperty('email');
            const isIdNumberType = typeof item.id === 'number';
            const isBodyStringType = typeof item.body === 'string';
            const isEmailStringType = typeof item.email === 'string';

            return isHasIdProperty && isHasBodyProperty && isHasEmailProperty && isIdNumberType && isBodyStringType && isEmailStringType;
          });
        }
      }
    }
  }
</script>

<style scoped>
</style>