<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref, type Ref } from 'vue';
import excerpts from './assets/data/excerpts.json';

function getRandomExcerpt(): [{ content: string[]; author: string }, number] {
    const excerpt = excerpts[Math.floor(Math.random() * excerpts.length)];
    return [excerpt, excerpts.indexOf(excerpt)];
}

function replaceToSpecifiedExcerpt(i: number) {
    history.pushState(null, '', `${document.location.href.split('?')[0]}?index=${i}`);
    [excerpt.value, index.value] = [excerpts[i], i];
}

function refresh() {
    let _index
    do {
        [, _index] = getRandomExcerpt();
    } while (_index === index.value);
    replaceToSpecifiedExcerpt(_index);
}

let index = ref(0);
let excerpt: Ref<{ content: string[]; author: string }> = ref({ content: [], author: '' });

onMounted(() => {
    const initExcerpt = () => {
        const params = new URL(document.location.href).searchParams;
        const _index = params.get('index');
        if (_index) {
            const i = parseInt(_index);
            if (i >= 0 && i < excerpts.length) {
                replaceToSpecifiedExcerpt(i);
                return;
            }
        }

        const [, i] = getRandomExcerpt();
        replaceToSpecifiedExcerpt(i);
    };

    initExcerpt();

    const handleKeyUp = (e: KeyboardEvent) => {
        switch (e.key) {
            case 'ArrowLeft':
                if (index.value > 0) replaceToSpecifiedExcerpt(index.value - 1);
                break;
            case 'ArrowRight':
                if (index.value < excerpts.length - 1) replaceToSpecifiedExcerpt(index.value + 1);
                break;
            case 'r':
                refresh();
                break;
            default:
                break;
        }
    };

    document.addEventListener('keyup', handleKeyUp);

    onBeforeUnmount(() => {
        document.removeEventListener('keyup', handleKeyUp);
    });
});
</script>

<template>
    <main>
        <header>
            <a href="https://github.com/Shikochin/excerpt.shikoch.in"><i class="fa-light fa-pen-nib"></i> 文摘</a>
        </header>
        <section id="excerpt">
            <p id="content">
                <span v-for="sentence in excerpt.content" :key="excerpt.content.indexOf(sentence)">{{ sentence
                    }}<br /></span>
            </p>
            <p id="author">—— {{ excerpt.author }}</p>
        </section>
        <section id="controller">
            <a title="上一个" v-if="index != 0" @click="replaceToSpecifiedExcerpt(index - 1)"><i
                    class="fa-thin fa-arrow-left-long"></i></a>
            <a class="disabled" title="已经到头了" v-else><i class="fa-thin fa-arrow-left-long"></i></a>
            <a title="随机" @click="refresh"><i class="fa-thin fa-arrow-rotate-right"></i></a>
            <a title="下一个" v-if="index != excerpts.length - 1" @click="replaceToSpecifiedExcerpt(index + 1)"><i
                    class="fa-thin fa-arrow-right-long"></i></a>
            <a class="disabled" title="已经到头了" v-else><i class="fa-thin fa-arrow-right-long"></i></a>
        </section>
        <footer>
            Excerpts are attributed to the author<br />
            2024-present by <a href="https://github.com/Shikochin">Shikochin</a>
        </footer>
    </main>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Instrument+Serif&family=Noto+Serif+SC&display=swap');

main {
    font-family: 'Instrument Serif', 'Noto Serif SC', serif;
}

header {
    font-size: 32px;
    margin-bottom: 8vh;
}

#excerpt {
    margin-bottom: 1vh;

    #content {
        font-size: 40px;
    }

    #author {
        font-size: 28px;
    }
}

#controller {
    font-size: 40px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10vh;

    a {
        opacity: 0.5;
        transition: opacity 0.3s;
    }

    a:hover {
        opacity: 0.8;
    }
}

a {
    text-decoration: none;
    color: black;
    cursor: pointer;
    user-select: none;
}

a.disabled {
    cursor: default;
    pointer-events: none;
    opacity: 0.2 !important;
}

footer {
    user-select: none;
    font-size: 22px;
}
</style>