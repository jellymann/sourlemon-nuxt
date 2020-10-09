<script>
  import GithubLogo from '../../static/github.svg';
  import articlePath from '../../functions/articlePath';

  export default {
    components: { GithubLogo },
    async asyncData({ $content, params }) {
      const projects = await $content('projects').fetch();
      return { projects };
    },
    methods: {
      toSentence(words) {
        if (!words || !words.length) return '';
        if (words.length === 1) return words[0];
        return `${words.slice(0, words.length-1).join(', ')} and ${words[words.length-1]}`;
      },
      projectImage(slug) {
        return require(`~/assets/project-images/${slug}.png`);
      },
      articlePath
    }
  }
</script>

<template>
  <div>
    <h1>Projects</h1>
    <div class="columns">
      <div
        v-for="project in projects" :key="project.name"
        class="column col-6 col-xs-12"
      >
        <div class="card">
          <div class="card-header">
            <div class="card-title">
              <h2 class="h5">{{ project.name }}</h2>
              
              <div class="project-tools">
                <span v-for="tool in project.tools" :key="tool" class="chip bg-primary">{{tool}}</span>
              </div>
            </div>
            <div class="card-subtitle text-gray">
              {{toSentence(project.tags)}}
            </div>
          </div>
          <div class="card-image">
            <img :src="projectImage(project.slug)" class="img-responsive">
          </div>
          <div class="card-body">
            <nuxt-content :document="project" />
          </div>
          <div class="card-footer">
            <a v-if="project.repo" class="btn" :href="project.repo" target="_blank">
              <GithubLogo />
              Source
            </a>
            <a v-if="project.website" class="btn" :href="project.website" target="_blank">
              <i class="icon icon-link"></i>
              Website
            </a>
            <n-link v-if="project.article" class="btn" :to="articlePath({ slug: project.article })">
              <i class="icon icon-link"></i>
              Article
            </n-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.btn {
  display: inline-flex;
  align-items: center;

  svg {
    display: block;
    width: 1rem;
    fill: currentColor;
  }

  svg, i {
    margin-right: 0.5rem;
  }
}

.card {
  border: 0;
  box-shadow: 0 0.25rem 1rem rgba(48,55,66,.15);
  height: 100%;
}

.column {
  margin-bottom: 1rem;
}

.card-title {
  display: flex;

  h2 {
    margin-right: 0.5rem;
  }
}

.project-tools {
  margin-left: auto;
  display: flex;
  justify-content: flex-end;
  flex-wrap: wrap;
}
</style>