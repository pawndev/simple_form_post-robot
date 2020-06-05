<template>
  <div>
    <b-card bg-variant="light">
      <b-form-group
        label-cols-lg="3"
        label="Anim-A'ssure"
        label-size="lg"
        label-class="font-weight-bold pt-0"
        class="mb-0"
      >
        <b-form-group
          label-cols-sm="3"
          label="Name:"
          label-align-sm="right"
          label-for="nested-street"
          :invalid-name="invalidName"
          :valid-name="validName"
          :state="state"
        >
          <b-form-input
            id="nested-street"
            v-model="name"
            :state="state"
            trim
          ></b-form-input>
        </b-form-group>

        <b-form-group
          v-model="animal"
          label-cols-sm="3"
          label="Animal:"
          label-align-sm="right"
          class="mb-0"
        >
          <b-form-radio-group
            class="pt-2"
            :options="options"
          ></b-form-radio-group>
        </b-form-group>
        <b-button variant="outline-primary" @click="validateForm"
          >Button</b-button
        >
      </b-form-group>
    </b-card>
  </div>
</template>

<script>
import postRobot from 'post-robot'

export default {
  data() {
    return {
      animal: '',
      name: '',
      options: ['dog', 'cat', 'seal']
    }
  },
  computed: {
    state() {
      return this.name.length >= 4
    },
    invalidName() {
      if (this.name.length > 4) {
        return ''
      } else if (this.name.length > 0) {
        return 'Enter at least 4 characters'
      } else {
        return 'Please enter something'
      }
    },
    validName() {
      return this.state === true ? 'Thank you' : ''
    }
  },
  mounted() {
    postRobot.on('fill_name', (event) => {
      this.name = event.data.name
      event.data.toggleModal()
    })
  },
  methods: {
    validateForm() {
      postRobot.send(parent.window, 'display_modal', {
        name: this.name,
        animal: this.animal,
        clearName: () => (this.name = '')
      })
      return true
    }
  }
}
</script>
