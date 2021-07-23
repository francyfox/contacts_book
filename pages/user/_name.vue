<template>
  <div class="wrap container is-max-desktop">
    <div class="column">
      <nav class="breadcrumb" aria-label="breadcrumbs">
        <ul>
          <li>
            <nuxt-link to="/">
            <span class="icon is-small">
              <fa icon="home"/>
            </span>
              <span>Back to home</span>
            </nuxt-link>
          </li>
          <li>
            <a @click.prevent="undo" href="#">
            <span class="icon is-small">
              <fa icon="undo"/>
            </span>
              <span>Undo last edit</span>
            </a>
          </li>
        </ul>
      </nav>
      <transition name="fade">
        <div v-if="danger" class="notification is-danger">
          <button @click="danger = false; count = 0" class="delete"></button>
          {{ msgTitle }} <br>
          {{ msgText }}
        </div>
      </transition>
      <div class="box content">
        <article class="post">
          <div class="media">
            <div class="media-content">
              <div class="content">
                <h1 class="title">
                  Name: {{ Storage[getID].name }}
                </h1>
                <div class="column is-6">
                  <table class="table" aria-describedby="user list paramters">
                    <thead>
                    <tr>
                      <th scope="col">Key</th>
                      <th scope="col">Value</th>
                      <th scope="col">Mod</th>
                    </tr>
                    </thead>
                      <tbody v-for="(value, name, index) in Storage[getID]" :key="index">
                        <tr v-if="name !== 'name'">
                          <th scope="row">{{ name }}</th>
                          <th scope="row">{{ value }}</th>
                          <th scope="row">
                            <a @click.prevent="editItem(name, value, index)" href="#" class="mr-2"><fa icon="edit"/></a>
                            <a @click.prevent="deleteItem(name)" href="#"><fa icon="trash"/></a>
                          </th>
                        </tr>
                      </tbody>
                  </table>
                </div>
                <div class="columns">
                  <div class="column is-2">
                    <div class="field">
                      <div class="control">
                        <input @keydown.enter="addAndChangeItem" v-model="inputKey" class="input" type="text" placeholder="Key">
                      </div>
                    </div>
                  </div>
                  <div class="column is-2">
                    <div class="field">
                      <div class="control">
                        <input @keydown.enter="addAndChangeItem" v-model="inputValue" class="input" type="text" placeholder="Value">
                      </div>
                    </div>
                  </div>
                  <div class="column is-3">
                    <a @click.prevent="clear" href="#" class="button is-small">CLEAR</a>
                    <a @click.prevent="addAndChangeItem" href="#" class="button is-link is-small">SEND</a>
                  </div>
                </div>
                <div class="title is-5">BACKUP</div>
              </div>
            </div>
          </div>
        </article>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
   return {
     msgTitle: '',
     msgText: '',
     count: 0,
     danger: false,
     inputKey: '',
     inputValue: '',
     inputIndex: 0,
     Storage: [],
     backupStorage: []
   }
  },
  created() {
    this.Storage = this.Info
  },
  computed: {
    getID () {
      return parseInt(this.$route.params.name)
    },
    Info: {
      get () {
        if (localStorage.contacts) {
          return JSON.parse(localStorage.contacts)
        }
        return []
      },
      set (newValue) {
        this.Storage = newValue
        localStorage.setItem('contacts', JSON.stringify(this.Storage))
      }
    }
  },
  methods: {
    clear () {
      this.count++;
      this.danger = true
      this.msgTitle = 'U want remove ME?'
      this.msgText = 'Oh nooo! Is this usefull notification? Probably nope'
      if (this.count === 2) {
        this.count = 0;
        this.danger = false
        this.inputKey = ''
        this.inputValue = ''
      }
    },

    undo () {
      this.Info = this.backupStorage
      localStorage.setItem('contacts', JSON.stringify(this.Info))
    },

    editItem (key, value, index) {
      this.inputValue = value
      this.inputKey = key
      this.inputIndex = index
    },

    deleteItem (key) {
      this.count++;
      this.danger = true
      this.msgTitle = 'Ahtung! Delete operation'
      this.msgText = 'Dont do this, if u dont know what are u want!'
      if (this.count === 2) {
        this.count = 0;
        this.danger = false
        this.backupStorage = JSON.parse(JSON.stringify(this.Storage))
        this.$delete(this.Storage[this.getID], key)
        localStorage.setItem('contacts', JSON.stringify(this.Storage))
      }
    },

    addAndChangeItem () {
      if (this.inputValue && this.inputKey) {
        this.backupStorage = JSON.parse(JSON.stringify(this.Storage))
        this.$set(this.Storage[this.getID], this.inputKey, this.inputValue)
        localStorage.setItem('contacts', JSON.stringify(this.Storage))
      }
    }
  }
}
</script>

<style scoped>

</style>
