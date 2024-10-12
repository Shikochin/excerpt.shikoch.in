<template>
    <section id="excerpt">
        <p id="content">
            <span v-for="(sentence, idx) in excerpt.content" :key="idx">{{ sentence }}<br /></span>
        </p>
        <p id="author">—— {{ excerpt.author }}</p>
    </section>
</template>

<script setup lang="ts">
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
    font-size: 4vw;
}

#author {
    font-size: 3vw;
}

@media (max-width: 600px) {
    #content {
        font-size: 8vw;
    }

    #author {
        font-size: 6vw;
    }
}

@media (min-width: 1200px) {
    #content {
        font-size: 3vw;
    }

    #author {
        font-size: 2vw;
    }
}
</style>
