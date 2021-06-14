<template>
  <div id="app">
    <div class="title-container">
      <img class="title-background" src="" alt="" />
      <p>Evgeny Povshedny's</p>
      <p>Playground</p>
    </div>
    <div class="container container--games">
      <p>Games</p>
      <div class="tabs">
        <div
          class="tabs-tab"
          :class="{ 'tabs-tab--active': currentGameTab === tab }"
          v-for="tab in gameTabs"
          :key="tab"
          @click="currentGameTab = tab"
        >
          {{ tab }}
        </div>
      </div>
      <keep-alive>
        <component v-bind:is="currentGameTab" />
      </keep-alive>
    </div>
  </div>
</template>

<script>
import MemoryPads from "./components/MemoryPads/MemoryPads";
import Snake from "@/components/Snake/Snake";

export default {
  name: "App",
  components: {
    MemoryPads,
    Snake,
  },
  data: () => {
    return {
      currentGameTab: "MemoryPads",
      gameTabs: ["MemoryPads", "Snake"],
    };
  },
  mounted() {
    setTimeout(() => {
      document.getElementById("app").classList.add("app--active");
    });
  },
};
</script>

<style lang="scss">
body {
  margin: 0;
  background: #222;
  height: 100%;
  width: 100%;
}

p {
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

  opacity: 0;
  transition: opacity 0.5s 0.3s;

  &.app--active {
    opacity: 1;

    .title-container p {
      opacity: 1;
      transition: opacity 0.5s 0.8s;
    }
  }
}

.title {
  &-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background: rgba(34, 34, 34, 0.5);

    p {
      opacity: 0;
      margin: 0;
      color: white;
      font-weight: bolder;
      font-size: 32px;

      &:nth-of-type(2) {
        font-size: 72px;
        text-transform: uppercase;
      }

      @media (max-width: 768px) {
        font-size: 22px;

        &:nth-of-type(2) {
          font-size: 52px;
        }
      }

      @media (max-width: 425px) {
        font-size: 16px;

        &:nth-of-type(2) {
          font-size: 38px;
        }
      }
    }
  }

  &-background {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
    z-index: -1;
    background-image: url(./assets/title_background.jpg);
    background-size: cover;
    filter: blur(10px);
  }
}

.tabs {
  display: flex;
  margin-bottom: 20px;
  overflow: auto;

  &-tab {
    padding: 20px;
    font-size: 20px;
    font-weight: bolder;
    text-transform: uppercase;

    &:hover {
      cursor: pointer;
    }

    &--active {
      border-bottom: 1px solid white;
    }
  }
}

.container {
  display: flex;
  flex-direction: column;
  padding: 50px;
  background: black;
  color: white;

  & > p {
    align-self: center;
    font-size: 50px;
  }
}

.menu-btn {
  display: flex;
  align-items: center;
  padding: 0 26px;
  color: #333;
  font-size: 16px;
  font-weight: bold;
  border-radius: 3px;
  border: 1px solid #333;
  background: #dfdfdf;
  height: 46px;
  text-transform: uppercase;
  cursor: pointer;

  margin-top: 20px;
}
</style>
