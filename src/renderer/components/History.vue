<template>
  <div class="history">
    <div v-if="messages.length===0" class="empty">
      <p :style="{ 'color': fontColorNormal }">
        {{ 'history.empty'|trans }}
      </p>
    </div>
    <div v-for="message in messages" :key="message.id" class="message">
      <span v-if="showMessageTitle" class="title" :style="{ color: fontColor(message) }">
        {{ getTitle(message) }}
      </span>
      <span v-if="showMessageSubtitle" class="subtitle" :style="{ color: fontColor(message) }">
        {{ getSubtitle(message) }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'History',
  props: {
    messages: {
      required: true
    },
    fontColorNormal: {
      type: String,
      default: '#000000'
    },
    fontColorPriority: {
      type: String,
      default: '#FF0000'
    },
    showMessageTitle: {
      type: Boolean,
      default: true
    },
    showMessageSubtitle: {
      type: Boolean,
      default: true
    },
    showMessageDescription: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    fontColor (message) {
      const peso = message.$data ? message.$data.peso : 0
      return peso > 0 ? this.fontColorPriority : this.fontColorNormal
    },
    getTitle (message) {
      return message.$data && message.$data.nomeCliente
        ? message.$data.nomeCliente.split(' ')[0]
        : message.title
    },
    getSubtitle (message) {
      if (message.$data && message.$data.nomeCliente) {
        return `${message.subtitle} - ${message.title}`
      }
      return message.subtitle
    }
  }
}
</script>
