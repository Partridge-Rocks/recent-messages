<template>
    <card class="flex flex-col divide-y">
        <!-- Loop through messages -->
        <div v-for="message in messages" :key="message.id" class="p-4 cursor-pointer">
            <!-- Display user name and thread name -->
            <div class="flex justify-between items-center">
                <span class="text-sm font-semibold">{{ message.user_name }}</span>
                <span class="text-xs text-gray-500">{{ message.thread_name }}</span>
            </div>
            <!-- Date of the message -->
            <div class="text-xs text-gray-600">{{ message.created_at }}</div>
            <!-- Message content area with Markdown rendering -->
            <div :class="{'max-h-40': !message.expanded, 'h-auto': message.expanded}" @click="toggleExpand(message)" class="mt-2 p-3 bg-blue-100 rounded-lg shadow text-blue-900 overflow-scroll transition-max-height duration-300 ease-in-out">
                <div v-html="renderMarkdown(message.content)"></div>
            </div>
            <!-- Text input for responding -->
            <input type="text" v-model="message.reply" placeholder="Reply to this message" class="mt-2 p-2 rounded-lg border border-gray-300 focus:outline-none focus:ring focus:ring-blue-200">
            <button @click="sendReply(message)" class="mt-2 px-4 py-2 bg-blue-500 text-white rounded-lg">Send Reply</button>
        </div>
    </card>
</template>

<script>
import MarkdownIt from 'markdown-it';

export default {
    props: ['card'],
    data() {
        return {
            md: new MarkdownIt()
        };
    },
    computed: {
        messages() {
            return this.card.messages || [];
        }
    },
    methods: {
        sendReply(message) {
            // Handle sending reply functionality here
            console.log("Reply sent to: ", message.id);
            console.log("Reply Content: ", message.reply);
        },
        toggleExpand(message) {
            message.expanded = !message.expanded;
        },
        renderMarkdown(content) {
            return this.md.render(content);
        }
    }
}
</script>

<style>
/* Scrollable area height */
.max-h-40 {
    max-height: 10rem; /* Adjust as needed */
}
</style>
