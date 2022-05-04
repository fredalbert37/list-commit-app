<template>
  <div class="list-items">
    <transition name="switch" mode="out-in">
      <div v-if="commits.length > 0">
        <transition-group tag="ul" name="list" appear>
          <li v-for="commit in commits" :key="commit.sha">
            <div style="display: flex; flex-wrap: wrap; flex-direction: row; align-items:center; justify-content:space-around; gap: 10px; font-size: .8rem">
              <div style="text-align: center;">
                <strong>
                ID:
                </strong> 
                {{commit.sha}}
              </div>
              <div style="text-align: center; padding: 5px;">
                <strong>Date:</strong> {{commit.date}}
              </div>
            </div>
            <div>
              <div style="display: flex; flex-wrap: wrap; align-items:center; justify-content: center; margin: 20px 0; font-size: .8rem">
                <strong>Message:</strong>
                <p style="text-transform: capitalize; margin: 5px 0; margin-left: 5px; "> 
                  {{commit.message}}
                </p>
              </div>
            </div>
            <div style="display: flex; align-items:center; justify-content: center; margin: 0; padding: 0; font-size: .9rem">
              <strong>Author:</strong>
              <p style="margin-left: 5px">{{commit.author}}</p>
            </div>
          </li>
        </transition-group>
      </div >
      <div v-else>Whoops, there are no commits in this repo! 😢</div>
    </transition>
  </div>
</template>
<script>
import { ref } from "vue";

export default {
  setup() {
    const commits = ref([]);

    return { commits };
  },
  methods: {
    async getCommits () {
      const data = await fetch('http://localhost:3001/api/v1/commits/1/2022-05-03');
      const jsonData = await data.json();
      this.commits = jsonData
    },
  },
  mounted(){
    this.getCommits();
  }
};
</script>
<style>
  .list-items{
    max-width: 700px;
    margin: 20px auto;
    position: relative;
  }

  .list-items ul{
    position: relative;
    padding: 0;
  }

  .list-items li {
    list-style-type: none;
    display: block;
    margin-bottom: 10px;
    padding: 1rem;
    background: white;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
    border-radius: 10px;
    width: 100%;
    box-sizing: border-box;
  }

  .list-items li:hover{
    cursor: pointer;
  }

  .list-enter-from{
    opacity: 0;
    transform: scale(0.6);
  }

  .list-enter-active{
    transition: all 0.4s ease;
  }

  .list-leave-to{
    opacity: 0;
    transform: scale(0.6);
  }

  .list-leave-active {
    transition: all 0.4s ease;
    position: absolute;
  }

</style>
