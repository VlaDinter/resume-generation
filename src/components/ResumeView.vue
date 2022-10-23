<template>
  <div class="card card-w70">
    <template v-if="blocks.length">
      <component
          v-for="block in blocks"
          :key="block.id"
          :is="`resume-${block.type}`"
          v-bind="{ value: block.value }"
      ></component>
    </template>

    <h3 v-else>Resume is empty</h3>
  </div>
</template>

<script>
  import ResumeTitle from './parts/ResumeTitle';
  import ResumeAvatar from './parts/ResumeAvatar';
  import ResumeSubtitle from './parts/ResumeSubtitle';
  import ResumeText from './parts/ResumeText';

  export default {
    props: {
      blocks: {
        type: Array,
        required: true,
        validator(value) {
          return value.every(item => {
            if (typeof item !== 'object' || item === null) {
              return false;
            }

            const isHasIdProperty = item.hasOwnProperty('id');
            const isHasTypeProperty = item.hasOwnProperty('type');
            const isHasValueProperty = item.hasOwnProperty('value');
            const isIdNumberType = typeof item.id === 'number';
            const isTypeStringType = typeof item.type === 'string';
            const isValueStringType = typeof item.value === 'string';

            return isHasIdProperty && isHasTypeProperty && isHasValueProperty && isIdNumberType && isTypeStringType && isValueStringType;
          });
        }
      }
    },

    components: {
      ResumeTitle,
      ResumeAvatar,
      ResumeSubtitle,
      ResumeText
    }
  }
</script>