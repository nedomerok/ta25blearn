
<script setup>
import { computed, ref } from 'vue'

const nextId = ref(4)
const newText = ref('')
const importance = ref('medium')

const items = ref([
  { id: 1, text: 'Learn Vue', isDone: false, importance: 'high' },
  { id: 2, text: 'Use Bulma Box', isDone: false, importance: 'medium' },
  { id: 3, text: 'Check the project', isDone: true, importance: 'low' }
])

const importanceOrder = {
  high: 3,
  medium: 2,
  low: 1
}

function sortByImportance(list) {
  return [...list].sort(
    (a, b) => importanceOrder[b.importance] - importanceOrder[a.importance]
  )
}

const sortedItems = computed(() => sortByImportance(items.value))

const doneItems = computed(() =>
  sortByImportance(items.value.filter(item => item.isDone))
)

const toDoItems = computed(() =>
  sortByImportance(items.value.filter(item => !item.isDone))
)

function add() {
  const text = newText.value.trim()

  if (text !== '') {
    items.value.push({
      id: nextId.value++,
      text,
      isDone: false,
      importance: importance.value
    })
  }

  newText.value = ''
}
</script>

<template>
  <main class="section">
    <div class="container app-shell">
      <header class="mb-5">
        <p class="eyebrow">MY PROJECT · VUE + BULMA</p>
        <h1 class="title is-3">My Shopping List</h1>
        <p class="subtitle is-6">
          Add items, choose importance and mark completed items.
        </p>
      </header>

      <form class="box add-box mb-5" @submit.prevent="add">
        <label class="label" for="item">New item</label>

        <div class="field has-addons">
          <div class="control is-expanded">
            <input
              id="item"
              v-model="newText"
              class="input"
              placeholder="Enter an item..."
            >
          </div>

          <div class="control">
            <div class="select">
              <select v-model="importance" aria-label="Importance">
                <option value="high">High</option>
                <option value="medium">Medium</option>
                <option value="low">Low</option>
              </select>
            </div>
          </div>

          <div class="control">
            <button class="button is-success" type="submit">
              Add item
            </button>
          </div>
        </div>
      </form>

      <section class="mb-5">
        <h2 class="title is-5">All Items</h2>
        <div
          v-for="item in sortedItems"
          :key="item.id"
          class="box task-box"
          :class="`priority-${item.importance}`"
        >
          <label class="checkbox task-label">
            <input v-model="item.isDone" type="checkbox">
            <span>{{ item.text }}</span>
          </label>
          <span class="tag" :class="{
            'is-danger': item.importance === 'high',
            'is-warning': item.importance === 'medium',
            'is-success': item.importance === 'low'
          }">
            {{ item.importance }}
          </span>
        </div>
      </section>

      <section class="mb-5">
        <h2 class="title is-5">Done Items</h2>
        <div
          v-for="item in doneItems"
          :key="item.id"
          class="box task-box"
          :class="`priority-${item.importance}`"
        >
          <label class="checkbox task-label">
            <input v-model="item.isDone" type="checkbox">
            <span>{{ item.text }}</span>
          </label>
          <span class="tag is-success">Done</span>
        </div>
      </section>

      <section>
        <h2 class="title is-5">ToDo Items</h2>
        <div
          v-for="item in toDoItems"
          :key="item.id"
          class="box task-box"
          :class="`priority-${item.importance}`"
        >
          <label class="checkbox task-label">
            <input v-model="item.isDone" type="checkbox">
            <span>{{ item.text }}</span>
          </label>
          <span class="tag" :class="{
            'is-danger': item.importance === 'high',
            'is-warning': item.importance === 'medium',
            'is-success': item.importance === 'low'
          }">
            {{ item.importance }}
          </span>
        </div>
      </section>
    </div>
  </main>
</template>
