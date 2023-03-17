<template>
  <div class="flex items-center justify-center flex-col bg-gray-400 2xl:w-[2006px] xl:w-[1350px] lg:w-[1094px] md:w-[838px] w-[768px]">
    <div class="">
      <h1 class="text-[50px] font-black font-mono">Anthony Samson</h1>
    </div>
    <div class=" 2xl:w-[1606px] xl:w-[1350px] lg:w-[1094px] md:w-[838px] w-[768px] flex flex-row  bg-gray-700 rounded-[10px] flex-wrap items-center justify-center shadow-xl shadow-black opacity-[50px]">
      <div
        class="flex flex-col items-center bg-gray-400 justify-center p-[15px] shadow-lg shadow-white opacity-[20px]  w-[500px] m-[50px]  rounded-[10px] border-black"
        v-for="repo in paginatedRepos"
        :key="repo.id"
      >
        <div class="flex items-center w-[450px]  justify-between ">
          <img />
          <router-link
          class="mr-[250px] font-mono"
            :to="{ name: 'Repository', params: { name: repo.name } }"
            >{{ repo.name }}</router-link
          >
         <div class="border-black border-[2px] w-[100px] flex items-center justify-center rounded-[15px] shadow-lg shadow-black opacity-[20px]">
          <span class="font-mono" v-if="repo.private ? 'Private' : 'Public'">
            {{ repo.private ? 'Private' : 'Public' }}
          </span>
          </div>
        </div>

        <div class="flex w-[400px] justify-between mt-[50px] ml-[50px] mr-[50px] font-mono">
          <span v-if="repo.language">
            {{ repo.language }}
          </span>
           
           <div class="flex">
            <svg
              fill="#000000"
              width="20px"
              height="20px"
              viewBox="0 0 32 32"
              version="1.1"
              xmlns="http://www.w3.org/2000/svg"
            >
              <title>star</title>
              <path
                d="M30.383 12.699c-0.1-0.303-0.381-0.519-0.713-0.519-0 0-0 0-0 0h-9.898l-3.059-9.412c-0.124-0.276-0.396-0.464-0.713-0.464s-0.589 0.189-0.711 0.459l-0.002 0.005-3.059 9.412h-9.897c-0.414 0-0.749 0.336-0.749 0.75 0 0.248 0.121 0.469 0.307 0.605l0.002 0.001 8.007 5.818-3.059 9.412c-0.023 0.070-0.037 0.15-0.037 0.233 0 0.414 0.336 0.75 0.75 0.75 0.165 0 0.318-0.054 0.442-0.144l-0.002 0.001 8.008-5.818 8.006 5.818c0.122 0.090 0.275 0.144 0.441 0.144 0.414 0 0.75-0.336 0.75-0.75 0-0.083-0.014-0.164-0.039-0.239l0.002 0.005-3.059-9.412 8.010-5.818c0.188-0.138 0.308-0.357 0.308-0.605 0-0.083-0.014-0.163-0.038-0.238l0.002 0.005zM20.779 18.461c-0.188 0.138-0.309 0.358-0.309 0.607 0 0.083 0.014 0.163 0.039 0.238l-0.002-0.005 2.514 7.736-6.581-4.783c-0.122-0.089-0.275-0.143-0.44-0.143s-0.318 0.053-0.443 0.144l0.002-0.002-6.581 4.783 2.514-7.736c0.024-0.070 0.037-0.15 0.037-0.233 0-0.249-0.121-0.469-0.307-0.605l-0.002-0.001-6.58-4.78h8.134c0 0 0.001 0 0.001 0 0.331 0 0.612-0.215 0.71-0.513l0.002-0.005 2.514-7.735 2.514 7.735c0.1 0.303 0.381 0.519 0.713 0.519 0 0 0 0 0 0h8.135z"
              ></path>
            </svg>
          <span v-if="repo.stargazers_count">
            {{ repo.stargazers_count }}
          </span>
          </div>
         
        </div>
      </div>
    </div>

      <div class="">
    <ul v-if="pageCount > 1" class="flex ">
      <li class="bg-gray-200 w-[50px] h-[50px] rounded-[30px] m-[30px] flex align-center justify-center  shadow-lg shadow-black opacity-[20px]" v-for="n in pageCount" :key="n">
        <router-link
          class="mt-[15px]"
          :to="{ name: 'Repositories', query: { page: n } }"
          @click="updateCurrentPage(n)"
          >{{ n }}</router-link
        >
      </li>
    </ul>
    </div>
  </div>
</template>

<script type="text/javascript">
export default {
  name: 'github-repos',
  data() {
    return {
      repositories: [],
      pagination: {
        perPage: 6,
        currentPage: 1,
      },
    }
  },
  created() {
    this.fetchGitHubRepos()
  },
  methods: {
    fetchGitHubRepos() {
      fetch(`https://api.github.com/users/Samson063/repos`)
        .then((response) => response.json())
        .then((data) => {
          this.repositories = data
        })
    },
    updateCurrentPage(page) {
      this.pagination.currentPage = page
    },
  },
  computed: {
    pageCount() {
      return Math.ceil(this.repositories.length / this.pagination.perPage)
    },
    paginatedRepos() {
      const startIndex =
        (this.pagination.currentPage - 1) * this.pagination.perPage
      const endIndex = startIndex + this.pagination.perPage
      return this.repositories.slice(startIndex, endIndex)
    },
  },
}
</script>
