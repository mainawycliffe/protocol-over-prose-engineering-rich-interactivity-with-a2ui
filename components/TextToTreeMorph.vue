<script setup lang="ts">
import { ref, onMounted } from 'vue'

const stage = ref(0)

onMounted(() => {
  const steps = [600, 600, 600, 900]
  let i = 0
  const tick = () => {
    stage.value = i % 4
    setTimeout(() => {
      i++
      tick()
    }, steps[i % 4])
  }
  tick()
})
</script>

<template>
  <svg viewBox="0 0 480 200" class="morph">
    <defs>
      <linearGradient id="g1" x1="0" x2="1" y1="0" y2="0">
        <stop offset="0%" stop-color="#00d486" />
        <stop offset="100%" stop-color="#7cdfff" />
      </linearGradient>
    </defs>

    <!-- Stage 0: raw text -->
    <text
      x="240" y="110"
      text-anchor="middle"
      class="prose"
      :class="{ visible: stage === 0 }"
      fill="#e6e8eb"
    >
      "Here is a table of users…"
    </text>

    <!-- Stage 1: JSON braces appear -->
    <text
      x="240" y="110"
      text-anchor="middle"
      class="json"
      :class="{ visible: stage === 1 }"
      fill="url(#g1)"
    >
      { "type": "table", "rows": [...] }
    </text>

    <!-- Stage 2: tree forms -->
    <g class="tree" :class="{ visible: stage >= 2 }">
      <circle cx="240" cy="40" r="6" fill="#00d486" />
      <line x1="240" y1="46" x2="170" y2="90" stroke="#00d486" stroke-width="1.2" />
      <line x1="240" y1="46" x2="240" y2="90" stroke="#00d486" stroke-width="1.2" />
      <line x1="240" y1="46" x2="310" y2="90" stroke="#00d486" stroke-width="1.2" />
      <circle cx="170" cy="96" r="5" fill="#7cdfff" />
      <circle cx="240" cy="96" r="5" fill="#7cdfff" />
      <circle cx="310" cy="96" r="5" fill="#7cdfff" />
      <line x1="170" y1="102" x2="140" y2="140" stroke="#7cdfff" stroke-width="1" opacity="0.7" />
      <line x1="170" y1="102" x2="200" y2="140" stroke="#7cdfff" stroke-width="1" opacity="0.7" />
      <line x1="310" y1="102" x2="280" y2="140" stroke="#7cdfff" stroke-width="1" opacity="0.7" />
      <line x1="310" y1="102" x2="340" y2="140" stroke="#7cdfff" stroke-width="1" opacity="0.7" />
      <circle cx="140" cy="146" r="3.5" fill="#ffb86b" />
      <circle cx="200" cy="146" r="3.5" fill="#ffb86b" />
      <circle cx="280" cy="146" r="3.5" fill="#ffb86b" />
      <circle cx="340" cy="146" r="3.5" fill="#ffb86b" />
    </g>
  </svg>
</template>

<style scoped>
.morph {
  width: 100%;
  max-width: 480px;
  height: auto;
  display: block;
  margin: 0 auto;
}

.prose,
.json {
  font-family: 'JetBrains Mono', ui-monospace, monospace;
  font-size: 18px;
  opacity: 0;
  transition: opacity 350ms ease;
}

.prose.visible,
.json.visible {
  opacity: 1;
}

.tree {
  opacity: 0;
  transform: translateY(8px);
  transition: opacity 500ms ease, transform 500ms ease;
}

.tree.visible {
  opacity: 1;
  transform: translateY(0);
}
</style>
