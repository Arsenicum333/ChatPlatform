<template>
  <q-page class="flex row no-wrap fit">
    <div class="channels-sidebar bg-grey-2 column">
      <div class="q-pa-md text-weight-bold text-subtitle1 row items-center justify-between border-bottom">
        <span class="ellipsis">Some Workspace</span>
        <q-btn flat round dense icon="expand_more" size="sm" />
      </div>

      <q-scroll-area class="col">
        <q-list padding dense class="text-grey-8">
          <div class="row items-center justify-between q-px-sm q-pb-xs">
            <span class="text-uppercase text-weight-bolder text-caption text-grey-7">
                Text Channels
            </span>
            <q-btn
              flat
              round
              dense
              icon="add"
              size="xs"
              color="grey-7"
              @click="openCreateChannelDialog"
            >
              <q-tooltip>Create Channel</q-tooltip>
            </q-btn>
          </div>

          <q-item
            v-for="channel in textChannels"
            :key="channel.id"
            clickable
            v-ripple
            :active="activeChannel === channel.id"
            active-class="bg-grey-4 text-primary text-weight-bold"
            class="rounded-borders q-mx-xs q-mb-xs"
            @click="activeChannel = channel.id"
          >
            <q-item-section avatar class="min-width-auto q-pr-sm">
              <q-icon name="tag" size="xs" />
            </q-item-section>
            <q-item-section class="ellipsis">
              {{ channel.name }}
            </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>
    </div>

    <div class="col column bg-white">
      <div class="q-px-md q-py-sm border-bottom row items-center justify-between">
        <div class="row items-center">
          <q-icon name="tag" size="sm" class="q-mr-xs text-grey-7" />
          <span class="text-weight-bold text-subtitle1">{{ currentChannelName }}</span>
        </div>
        <div>
          <q-btn flat round dense icon="notifications" color="grey-7" />
          <q-btn flat round dense icon="people" color="grey-7" />
        </div>
      </div>

      <q-scroll-area class="col q-pa-md">
        <div v-for="msg in messages" :key="msg.id" class="q-mb-md">
          <div class="row items-center q-mb-xs">
            <q-avatar size="28px" color="amber-8" text-color="white" class="q-mr-sm">
              {{ msg.author[0] }}
            </q-avatar>
            <span class="text-weight-bold q-mr-sm">{{ msg.author }}</span>
            <span class="text-caption text-grey-6">{{ msg.time }}</span>
          </div>
          <div class="q-pl-xl text-body2 text-grey-9">
            {{ msg.text }}
          </div>
        </div>
      </q-scroll-area>

      <div class="q-pa-md border-top">
        <q-input
          v-model="newMessage"
          dense
          outlined
          rounded
          :placeholder="`Write in #${currentChannelName}...`"
          @keyup.enter="sendMessage"
        >
          <template v-slot:append>
            <q-btn round flat icon="send" color="primary" @click="sendMessage" />
          </template>
        </q-input>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref, computed } from 'vue'

const activeChannel = ref('gen')
const newMessage = ref('')

const textChannels = ref([
  { id: 'gen', name: 'general' },
  { id: 'ann', name: 'announcements' },
  { id: 'dev', name: 'developers' },
  { id: 'random', name: 'random' }
])

const currentChannelName = computed(() => {
  return textChannels.value.find(c => c.id === activeChannel.value)?.name || 'general'
})

const messages = ref([
  { id: 1, author: 'Alex', text: 'Welcome to Nexum General Chat!', time: '12:30' },
  { id: 2, author: 'Mariya', text: 'Hello everyone! How is the development going?', time: '12:32' }
])

function sendMessage() {
  if (!newMessage.value.trim()) return
  messages.value.push({
    id: Date.now(),
    author: 'You',
    text: newMessage.value,
    time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
  })
  newMessage.value = ''
}

function openCreateChannelDialog() {

}
</script>

<style scoped>
.channels-sidebar {
  width: 240px;
  min-width: 240px;
  border-right: 1px solid #e0e0e0;
}

.min-width-auto {
  min-width: auto;
}

.border-bottom {
  border-bottom: 1px solid #e0e0e0;
}

.border-top {
  border-top: 1px solid #e0e0e0;
}

.leading-tight {
  line-height: 1.2;
}
</style>
