<template>
  <div id="app">
    <tarefas></tarefas>
  </div>
</template>

<script>
import Tarefas from './components/Hello.vue'
import moment from 'moment-timezone'

export default {
  name: 'app',
  components: {
    Tarefas, moment
  }
}
</script>
