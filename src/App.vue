<script setup lang="ts">
import { onMounted, ref, type Ref } from 'vue';
import excerpts from './assets/excerpts.json';

function getRandomExcerpt(): [{ content: string[]; author: string }, number] {
  const excerpt = excerpts[Math.floor(Math.random() * excerpts.length)];
  return [excerpt, excerpts.indexOf(excerpt)];
}

function replaceToSpecifiedExcerpt(i: number) {
  history.pushState(null, '', `${document.location.href.split('?')[0]}?index=${i}`);
  [excerpt.value, index.value] = [excerpts[i], i];
}

function refresh() {
  const [, _index] = getRandomExcerpt();
  if (_index === index.value) {
    refresh();
  } else {
    replaceToSpecifiedExcerpt(_index);
  }
}

let index = ref(0);
let excerpt: Ref<{ content: string[]; author: string }> = ref({ content: [], author: '' });

onMounted(() => {
  const params = new URL(document.location.href).searchParams;
  const _index = params.get('index');
  if (_index) {
    const __index = parseInt(_index);
    if (__index >= 0 && __index <= excerpts.length - 1) {
      replaceToSpecifiedExcerpt(__index);
    } else {
      const [, index] = getRandomExcerpt();
      replaceToSpecifiedExcerpt(index);
    }
  } else {
    const [, index] = getRandomExcerpt();
    replaceToSpecifiedExcerpt(index);
  }
});
</script>

<template>
  <main>
    <header><a id="random" title="刷新" @click="refresh">文摘 | excerpt.shikoch.in</a></header>
    <section id="excerpt">
      <p id="content">
        <span v-for="sentence of excerpt.content" :key="excerpt.content.indexOf(sentence)">{{ sentence }}<br /></span>
      </p>
      <p id="author">—— {{ excerpt.author }}</p>
    </section>
    <section id="controller">
      <a title="上一个" v-if="index != 0" @click="replaceToSpecifiedExcerpt(index - 1)">←</a>
      <a class="disabled" title="已经到头了" v-else>←</a>
      <a title="下一个" v-if="index != excerpts.length - 1" @click="replaceToSpecifiedExcerpt(index + 1)">→</a>
      <a class="disabled" title="已经到头了" v-else>→</a>
    </section>
    <footer>
      <a href="https://github.com/Shikochin"><i class="fa-brands fa-github"></i></a>
      <a href="https://x.com/shikoch1n"><i class="fa-brands fa-x-twitter"></i></a>
      <a href="mailto:i@shikoch.in"><i class="fa-solid fa-envelope"></i></a>
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
  font-size: 40px;
  a {
    margin-right: 30px;
  }
}
</style>
