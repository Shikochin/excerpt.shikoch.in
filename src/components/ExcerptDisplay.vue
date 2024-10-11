<template>
    <section id="excerpt">
        <p id="content">
            <span v-for="(sentence, idx) in excerpt.content" :key="idx">{{ sentence }}<br /></span>
        </p>
        <p id="author">—— {{ excerpt.author }}</p>
    </section>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';

const props = defineProps<{
    excerpt: {
        content: string[];
        author: string;
    };
}>();
</script>

<style scoped>
#excerpt {
    margin-bottom: 1vh;
}

#content {
    font-size: 40px;
}

#author {
    font-size: 28px;
}
</style>