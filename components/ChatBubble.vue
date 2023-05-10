<script lang="ts" setup>
import { Message, User } from "../types";
import Markdown from "vue3-markdown-it";

const props = defineProps<{
    user?: User
    message?: Message
    isMine?: boolean
}>()

const relativeTime = useTimeAgo(() => props.message?.createdAt || new Date())

</script>

<template>
    <div 
        class="chat" 
        :class="{
            'chat-end': isMine, 
            'chat-start': !isMine 
        }">
        <div class="chat-image avatar">
            <div class="w-10 rounded-full">
                <img :src="user?.avatar" alt="" >
            </div>
        </div>

        <div class="chat-header mb-2">
            {{  user?.name }}
            <time v-if="message" class="text-xs opacity-50">{{
                relativeTime
            }}</time>
        </div>

        <div class="chat-bubble max-w-max w-full bg-white dark:bg-gray-900 prose prose-sm" :class="{
        'dark:bg-gray-700 bg-gray-600 dark:text-inherit text-white': isMine,
      }">
            <Markdown v-if="message" :source="message?.text" class="w-full"/>

            <slot v-else />
        </div>
    </div>
</template>  