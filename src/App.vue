<template>
  <div>
    <NavBar/>
    <component :is = "currentPage" style = "padding-top: 56.67px;"></component>
  </div>
</template>

<script>
import IndexPage from "./pages/index.vue"
import PersonalityQuiz from "./pages/PersonalityQuiz.vue"
import QuizResult from "./pages/QuizResult.vue"
import NavBar from "./components/NavBar.vue"
import PasswordGen from "./pages/PasswordGen.vue"
import WeatherPage from "./pages/weather/Weather.vue"
import AgendaPage from "./pages/Agenda.vue"
const routes = {
  "/": IndexPage,
  "/quiz": PersonalityQuiz,
  "/quiz-result": QuizResult,
  "/password-gen": PasswordGen,
  "/weather": WeatherPage,
  "/agenda": AgendaPage
}
export default {
  name: 'App',
  components: {
      NavBar
  },
data() {
  return {
    currentPath: window.location.pathname
  }
},
computed: {
  currentPage() {
    return routes[this.currentPath || "/"]
  }
}
}
</script>

<style>
  
</style>
