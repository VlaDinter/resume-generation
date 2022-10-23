<template>
  <div class="container column">
    <ResumeForm @block-added="addBlock" />
    <AppLoader v-if="resumeLoading" />
    <ResumeView :blocks="blocks" v-else />
  </div>

  <div class="container">
    <AppLoader v-if="loading" />
    <ResumeComments
        v-else
        :comments="comments"
        @load-comments="loadComments"
    />
  </div>
</template>

<script>
  import AppLoader from './components/AppLoader';
  import ResumeComments from './components/ResumeComments';
  import ResumeForm from './components/ResumeForm';
  import ResumeView from './components/ResumeView';

  export default {
    components: {
      AppLoader,
      ResumeComments,
      ResumeForm,
      ResumeView
    },

    data() {
      return {
        comments: [],
        blocks: [],
        loading: false,
        resumeLoading: false
      };
    },

    mounted() {
      this.load();
    },

    methods: {
      async addBlock(block) {
        this.blocks.push(block);
        try {
          await fetch('https://vue-demo-deploy-30886-default-rtdb.europe-west1.firebasedatabase.app/blocks.json', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },

            body: JSON.stringify(block)
          });
        } catch(error) {
          console.error(error.message);
        }
      },

      async loadComments() {
        this.loading = true;
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42');

          this.comments = await response.json();
        } catch(error) {
          console.error(error.message);
        }

        this.loading = false;
      },

      async load() {
        this.resumeLoading = true;
        try {
          const response = await fetch('https://vue-demo-deploy-30886-default-rtdb.europe-west1.firebasedatabase.app/blocks.json');
          const data = await response.json();

          if (!data) {
            throw new Error('Resume is empty');
          }

          Object.keys(data).forEach(key => {
            this.blocks.push(data[key]);
          });
        } catch(error) {
          console.error(error.message);
        }

        this.resumeLoading = false;
      }
    }
  }
</script>
