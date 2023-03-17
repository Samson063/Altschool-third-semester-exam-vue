<template>
  <div class="flex flex-col items-center justify-center bg-gray-400 h-[full]">
    <h1 class="text-[50px] font-bold">Repository Details</h1>
    <ul v-if="Object.keys(repository).length !== 0">
      <li class="m-[50px] text-[20px] font-semibold">Name: {{ repository.name }}</li>
      <li class="m-[50px] text-[20px] font-semibold">Description: {{ repository.description }}</li>
      <li class="m-[50px] text-[20px] font-semibold">Stars: {{ repository.stargazers_count }}</li>
      <li class="m-[50px] text-[20px] font-semibold">Forks: {{ repository.forks_count }}</li>
      <li class="m-[50px] text-[20px] font-semibold">Language: {{ repository.language }}</li>
      <li class="m-[50px] text-[20px] font-semibold">Visibility: {{repository.private ? 'Private' : 'Public'}}</li>
      <button class="w-[200px] h-[50px] rounded-[10px] bg-gray-700 ml-[50px] text-white font-[semibold]">
        <a target="_blank" :href="`${repository.html_url}`"> Link to repo</a>
      </button>
    </ul>
    <p v-else>Loading...</p>
  </div>
</template>

<!-- Disable eslint rule for component name -->
<!-- eslint-disable vue/multi-word-component-names -->
<script type="text/javascript">
export default {
  name: "Repository",
  data() {
    return {
      repository: {},
    };
  },
  created() {
    const repositoryName = this.$route.params.name;
    fetch(`https://api.github.com/repos/Samson063/${repositoryName}`)
      .then((response) => response.json())
      .then((data) => {
        this.repository = data;
      });
  },
};
</script>
