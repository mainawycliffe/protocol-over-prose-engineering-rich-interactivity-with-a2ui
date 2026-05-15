<script setup lang="ts">
import { ref } from 'vue'

const tree = {
  type: 'card',
  title: 'Sign up for the beta',
  subtitle: 'Get early access to A2UI primitives.',
  fields: [
    { type: 'text', name: 'name', label: 'Full name', placeholder: 'Ada Lovelace' },
    { type: 'email', name: 'email', label: 'Email', placeholder: 'ada@example.com' },
    {
      type: 'select',
      name: 'role',
      label: 'Role',
      options: ['Engineer', 'Designer', 'PM', 'Other'],
    },
    { type: 'checkbox', name: 'updates', label: 'Send me product updates' },
  ],
  submit: { label: 'Request access', variant: 'primary' },
}

const submitted = ref(false)
const form = ref<Record<string, unknown>>({})

function handleSubmit() {
  submitted.value = true
  setTimeout(() => (submitted.value = false), 1800)
}
</script>

<template>
  <div class="demo-card">
    <header class="demo-header">
      <h3>{{ tree.title }}</h3>
      <p class="dim">{{ tree.subtitle }}</p>
    </header>

    <div class="demo-fields">
      <label v-for="field in tree.fields" :key="field.name" class="field">
        <span class="label">{{ field.label }}</span>

        <input
          v-if="field.type === 'text' || field.type === 'email'"
          :type="field.type"
          :placeholder="field.placeholder"
          v-model="form[field.name]"
        />

        <select v-else-if="field.type === 'select'" v-model="form[field.name]">
          <option disabled value="">Choose one…</option>
          <option v-for="opt in field.options" :key="opt" :value="opt">{{ opt }}</option>
        </select>

        <span v-else-if="field.type === 'checkbox'" class="check-row">
          <input type="checkbox" v-model="form[field.name]" />
          <span class="dim">opt-in</span>
        </span>
      </label>
    </div>

    <button class="submit" @click="handleSubmit">
      {{ submitted ? '✓ Submitted' : tree.submit.label }}
    </button>
  </div>
</template>

<style scoped>
.demo-card {
  background: #11151a;
  border: 1px solid #25292f;
  border-radius: 12px;
  padding: 1.5rem;
  font-family: 'Inter', sans-serif;
  color: #e6e8eb;
  width: 100%;
  max-width: 360px;
}

.demo-header h3 {
  font-family: 'Inter', sans-serif;
  font-size: 1.1rem;
  font-weight: 600;
  margin: 0 0 0.25rem 0;
  color: #ffffff;
}

.demo-header p {
  margin: 0 0 1.25rem 0;
  font-size: 0.85rem;
}

.dim { color: #8b94a3; }

.demo-fields {
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
  margin-bottom: 1.25rem;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.label {
  font-size: 0.75rem;
  color: #a8b1bf;
  letter-spacing: 0.02em;
}

input[type='text'],
input[type='email'],
select {
  background: #0b0d10;
  border: 1px solid #2a2f37;
  border-radius: 6px;
  padding: 0.55rem 0.7rem;
  color: #e6e8eb;
  font-family: 'Inter', sans-serif;
  font-size: 0.9rem;
  outline: none;
}

input:focus,
select:focus {
  border-color: #00d486;
}

.check-row {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.85rem;
}

.submit {
  background: #00d486;
  color: #07090c;
  font-family: 'JetBrains Mono', monospace;
  font-weight: 700;
  font-size: 0.9rem;
  border: none;
  border-radius: 6px;
  padding: 0.65rem 1rem;
  width: 100%;
  cursor: pointer;
  transition: transform 120ms ease, background 120ms ease;
}

.submit:hover {
  background: #2bf09d;
  transform: translateY(-1px);
}
</style>
