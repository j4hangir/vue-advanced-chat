<template>
  <transition-group v-if="!message.deleted" name="vac-slide-left" tag="span">
    <button
      v-for="(reaction, key) in message.reactions"
      v-show="reaction.length"
      :key="key + 0"
      class="vac-button-reaction"
      :class="{
				'vac-reaction-me': reaction.indexOf(currentUserId) !== -1
			}"
      :style="{
				float: message.senderId === currentUserId ? 'right' : 'left'
			}"
      @click="sendMessageReaction({ unicode: key }, reaction)"
    >
      {{ key }}
      <div>
        <div class="container">
          <img v-for="user in reactedUsers(reaction)" :key="user._id" :src="user.avatar" />
        </div>
      </div>
    </button>
  </transition-group>
</template>

<script>

export default {
  name: 'MessageReactions',

  props: {
    currentUserId: { type: [String, Number], required: true },
    users: { type: Array, default: () => [] },
    message: { type: Object, required: true },
  },

  emits: ['send-message-reaction'],

  methods: {
    reactedUsers(reaction) {
      return this.users.filter(u => reaction.indexOf(u._id.toString()) !== -1).sort(u => reaction.indexOf(u._id.toString()))
    },
    sendMessageReaction(emoji, reaction) {
      this.$emit('send-message-reaction', { emoji, reaction })
    },
  },
}
</script>
