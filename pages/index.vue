<template>
  <div class="wrap">
    <transition name="home" mode="out-in"/>
    <section class="container is-w">
      <div class="columns">
        <div class="column is-8">
          <nav class="panel is-link">
            <p class="panel-heading">
              Contacts
            </p>
            <p v-if="userList.length == 0">NO USERS</p>
            <template
              v-for="(user, index) in userList"
            >
              <div :key="index" class="is-flex is-justify-content-space-between is-align-items-center __link">
                <a @click.prevent="changeRouter(user, index)" href="#" class="panel-block" >
                  <span class="panel-icon">
                    <fa icon="user"></fa>
                  </span>
                  <span class="__column">
                    <div><strong>{{ user.name }}</strong></div>
                    <div>{{ user.number }}</div>
                  </span>
                </a>
                <a @click.prevent="deleteContact(index, 0)" href="#" class="trash">
                  <fa icon="trash"></fa>
                </a>
              </div>

            </template>
          </nav>
        </div>
        <div class="column is-8">
          <div class="box content">
            <article class="post">
              <transition name="fade">
              <div v-if="danger" class="notification is-danger">
                <button @click="danger = false; count = 0" class="delete"></button>
                A u sure about this? <br>
                Click again to trash if u wanna kill me
              </div>
              </transition>
              <h4>Add new contact</h4>
              <div class="media">
                <div class="media-content">
                  <div class="content">
                    <div class="field">
                      <label class="label">Name</label>
                      <div class="control">
                        <input v-model="name" class="input" type="text" placeholder="e.g Alex Smith">
                      </div>
                    </div>

                    <div class="field">
                      <label class="label">Phone</label>
                      <div class="control">
                        <input v-model="number" class="input" type="tel" placeholder="Phone Number">
                      </div>
                    </div>
                    <a @click.prevent="addContact" class="button is-link is-block is-alt is-small" href="#">New Contact</a>
                  </div>
                </div>
              </div>
            </article>
          </div>
        </div>
      </div>
    </section>
    <footer class="footer">
      <div class="container">
        <div class="content has-text-centered">
          <div class="columns is-mobile is-centered">
            <div class="field is-grouped is-grouped-multiline">
              <div class="control">
                <div class="tags has-addons">
                  <a class="tag is-link" href="https://github.com/francyfox">@francyfox</a>
                  <span class="tag is-light">Danil Golota</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  transitions: {
    name: 'home',
    mode: 'out-in'
  },

  data() {
    return {
      count: 0,
      danger: false,
      userList: [],
      name: '',
      number: ''
    }
  },
  created() {
    this.userList = JSON.parse(localStorage.contacts)
  },
  methods: {
    changeRouter (user, index) {
      const pathToRouter = '/user/'+index
      this.$router.push({ path: pathToRouter })
    },

    deleteContact (index) {
      this.count++;
      this.danger = true
      if (this.count === 2) {
        this.count = 0;
        this.danger = false
        this.userList.splice(index, 1)
        localStorage.setItem('contacts', JSON.stringify(this.userList))
      }
    },

    addContact () {
      this.userList.push({name: this.name, number: this.number})
      localStorage.setItem('contacts', JSON.stringify(this.userList))
    }

  }
}
</script>

<style>

</style>
