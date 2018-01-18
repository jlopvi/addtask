<template lang="pug">
  #app
    .header
      .row
        .col-12
          h1 Lista de tareas
            select(v-model="useractive")
              option(v-for="u in users", :value="u", :select="u == useractive ? true : false") {{ u | capitalize }}


    .container
      .row
        .col
          button.task-btn.--blue(@click="adding = true") Agregar tarea a {{useractive}}
          form.task-item(v-show="adding", @submit.prevent="addtask")
            div
              input(placeholder="Titulo", name="title")
            div
              input(placeholder="Descripcion de tarea", name="text")
            div
              input(placeholder="hora", name="time")
            div
              button.task-btn.--blue(type="submit") Agregar

      .row
        .col-12
          ul
            li.task-item(v-for="(t, i) in taskuseractive")
              div(v-show="!t.editing")
                h2.task-title {{ t.title }}

                p.task-text {{ t.text }}
                .task-footer
                  span.task-time {{ t.time }}
                  button.task-btn.--blue(@click="t.editing = true") Editar
              form(v-show="t.editing", @submit.prevent="updateTask")
                input(type="hidden", :value="i", name="id")
                div
                  label(:for="'title'+i") Titulo:
                  input(:value="tasks[useractive][i].title", :id="'title'+i", name="title")
                div
                  label(:for="'text'+i") Texto:
                  input(:value="tasks[useractive][i].text", :id="'text'+i", name="text")
                div
                  button.task-btn.--blue(type="submit") Guardar
                  button.task-btn.--red(type="submit", @click="t.editing = false") Cancelar
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      users: ['jesus', 'karlos', 'david'],
      useractive: 'jesus',
      taskuseractive: [],
      adding: false,
      tasks: {
        'jesus': [
          {
            id: 1,
            title: 'Titulo jesus 1',
            text: 'Texto jesus 1',
            time: '8:00',
            editing: false
          },
          {
            id: 2,
            title: 'Titulo jesus 2',
            text: 'Texto jesus 2',
            time: '8:00',
            editing: false
          },
          {
            id: 3,
            title: 'Titulo jesus 3',
            text: 'Texto jesus 3',
            time: '8:00',
            editing: false
          }
        ],
        'karlos': [
          {
            id: 1,
            title: 'Titulo karlos 1',
            text: 'Texto karlos 1',
            time: '8:00',
            editing: false
          },
          {
            id: 2,
            title: 'Titulo karlos 2',
            text: 'Texto karlos 2',
            time: '8:00',
            editing: false
          },
          {
            id: 3,
            title: 'Titulo karlos 3',
            text: 'Texto karlos 3',
            time: '8:00',
            editing: false
          }
        ],
        'david': [
          {
            id: 1,
            title: 'Titulo david 1',
            text: 'Texto david 1',
            time: '8:00',
            editing: false
          },
          {
            id: 2,
            title: 'Titulo david 2',
            text: 'Texto david 2',
            time: '8:00',
            editing: false
          },
          {
            id: 3,
            title: 'Titulo david 3',
            text: 'Texto david 3',
            time: '8:00',
            editing: false
          }
        ]
      }
    }
  },

  mounted: function () {
    this.taskuseractive = this.tasks[this.useractive]
  },

  methods: {
    updateTask: function (ev) {
      let id = ev.target.elements.id.value
      let newTitle = ev.target.elements.title.value
      let newText = ev.target.elements.text.value
      this.tasks[this.useractive][id].title = newTitle
      this.tasks[this.useractive][id].text = newText
      this.tasks[this.useractive][id].editing = false
    },
    addtask: function (ev) {
      let newTitle = ev.target.elements.title.value
      let newText = ev.target.elements.text.value
      let newTime = ev.target.elements.time.value
      let obj = {
        id: 0,
        title: newTitle,
        text: newText,
        time: newTime,
        editing: false
      }
      this.tasks[this.useractive].push(obj)
      ev.target.elements.title.value = ''
      ev.target.elements.text.value = ''
      ev.target.elements.time.value = ''
      this.adding = false
    }

  },

  filters: {
    capitalize (str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  },

  watch: {
    useractive () {
      this.taskuseractive = this.tasks[this.useractive]
    }
  }
}
</script>

<style lang="scss">
@import "./scss/main.scss";
</style>
