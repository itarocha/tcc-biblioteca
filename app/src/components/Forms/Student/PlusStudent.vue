<template>
  <div>
    <Hero title="Adicionar Aluno"></Hero>
    <section class="section">
      <div class="columns is-centered">
        <div class="column is-8">
          <FormInsertAluno
            v-if="open === true"
            @result="confirmar"></FormInsertAluno>
          <hr>
          <Notification
            :estado="estadoNotification"
            :array="dados"
            v-if="err"
            @closeNotification="close"></Notification>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { addStudent } from '../../../pouchdb/student'
import Hero from '../../Hero/Main'
import FormInsertAluno from './PlusStudent/FormInsertAluno'
import Notification from '../formComponents/Notification'

export default {
  name: 'form-plus-student',
  components: { Hero, FormInsertAluno, Notification },
  data () {
    return {
      err: false,
      open: true,
      estadoNotification: 'negative',
      dados: []
    }
  },
  methods: {
    confirmar () {
      this.err = true
      if (arguments[2]) {
        this.open = false
        const dados = arguments[0]
        this.estadoNotification = arguments[1]
        const student = {
          name: dados.Name.toUpperCase(),
          turma: dados.Turma
        }
        const self = this
        const result = addStudent(student)
        result.then(function (data) {
          // Capture the data
          if (data.ok) {
            self.dados = {
              Nome: dados.Name,
              Turma: dados.Turma
            }
          }
        })
      } else {
        this.open = false
        this.dados = []
        this.estadoNotification = 'negative'
      }
    },
    close () {
      this.err = false
      this.open = true
    }
  }
}
</script>
