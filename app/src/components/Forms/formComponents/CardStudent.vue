<template>
  <div class="card" style="width: 100%">
    <div class="card-header">
      <div class="card-header-title">
        <h2 class="title is-4"> Aluno: {{ object.name }} </h2>
      </div>
    </div>
    <div class="card-content">
      <p> Turma: {{ object.turma }} </p>
    </div>
    <div class="card-footer">
      <div class="card-footer-item">
        <button
          class="button is-fullwidth is-info"
          @click="showButton = true"
          v-if="!showButton"> Selecionar </button>
        <transition name="slide-fade">
          <button
            class="button is-fullwidth"
            :class="classCard"
            v-if="showButton"
            @click="returnDados"> {{ label }} </button>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['object', 'purpose'],
  data () { return { showButton: false } },
  computed: {
    classCard () {
      if (this.purpose === 'delete') return 'is-danger'
      if (this.purpose === 'search') return 'is-success'
    },
    label () {
      if (this.purpose === 'delete') return 'Confirmar Exclusão'
      if (this.purpose === 'search') return 'Confirmar Seleção'
    }
  },
  methods: {
    returnDados () {
      this.$emit('returnData', this.object._id, this.object._rev, this.object.name, this.object.turma, this.object.records)
    }
  }
}
</script>

<style scoped>
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for <2.1.8 */ {
  transform: translateY(-100%);
  opacity: 0;
}
</style>
