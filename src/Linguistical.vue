<script setup>
import { ref, onBeforeMount } from 'vue'
import { round } from './lib'

const props = defineProps({
  user: {
    type: String,
    required: true,
  },
  repo: {
    type: String,
    required: true,
  },
  theme: {
    validator(value) {
      return ['dark', 'light', ''].includes(value)
    },
    default: 'light',
  },
})

const languages = ref([])
const url = ref('')

onBeforeMount(async () => {
  const response = await fetch(
    `https://linguistiql.fly.dev/stats?user=${props.user}&repo=${props.repo}`
  )
  const data = await response.json()

  languages.value = data.languages
  url.value = data.url
})
</script>

<template>
  <div
    class="linguistical"
    :style="{
      '--linguistical-bg-color': theme === 'dark' ? '#2d333b' : 'transparent',
      '--linguistical-text-primary': theme === 'dark' ? '#cdd9e5' : '#1f2328',
      '--linguistical-text-secondary': theme === 'dark' ? '#768390' : '#656d76',
    }"
  >
    <div class="graph">
      <div v-if="!languages.length" class="skeleton"></div>

      <div
        v-for="{ name, color, size } in languages"
        :key="name"
        :style="{ width: `${size}%`, backgroundColor: color }"
      ></div>
    </div>

    <ul class="text">
      <li v-if="!languages.length">
        <a style="cursor: pointer">
          <span
            class="circle"
            :style="{ backgroundColor: 'var(--linguistical-text-secondary)' }"
          ></span>
          <span class="language">Code</span>
          <span class="size">100%</span>
        </a>
      </li>

      <li v-for="{ name, color, size } in languages" :key="name">
        <a :href="`${url}/search?l=${name}`">
          <span class="circle" :style="{ backgroundColor: color }"></span>
          <span class="language">
            {{ name }}
          </span>
          <span class="size">{{ round(size) }}%</span>
        </a>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.linguistical {
  width: 100%;
  padding: 14px 12px 12px 12px;
  background-color: var(--linguistical-bg-color);
  font-family: sans-serif;
}

.graph {
  display: flex;
  height: 8px;
  margin-bottom: 6px;
}

.skeleton {
  width: 100%;
  background-color: var(--linguistical-text-secondary);
}

.graph > * {
  margin-right: 2px;
}

.graph > *:first-child {
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
}

.graph > *:last-child {
  margin-right: 0;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
}

.text {
  display: flex;
  flex-wrap: wrap;
  font-size: 12px;
  margin: 0;
  padding: 0px;
  padding-left: 3px;
}

li {
  display: inline-flex;
  align-items: center;
  flex-wrap: nowrap;
  list-style: none;
  line-height: 21px;
  margin: 0;
  margin-right: 16px;
  padding-top: 5px;
}

a {
  text-decoration: none !important;
  color: unset;
  line-height: 18px;
}

a:hover,
a:hover .language,
a:hover .size {
  color: #539bf5 !important;
}

.circle {
  display: inline-block;
  width: 8px;
  height: 8px;
  margin-right: 7px;
  border-radius: 100%;
}

.language {
  font-weight: 600;
  margin-right: 4px;
  color: var(--linguistical-text-primary);
}

.size {
  color: var(--linguistical-text-secondary);
}
</style>
