<script setup>
import { ref } from "vue";

defineProps({
  todos: {
    type: Array,
    default() {
      return [];
    },
  },
});

const emit = defineEmits(["deleteByIndexes", "switchStatusByIndex"]);
const selectedIndexes = ref([]);

const batchDelete = () => {
  emit("deleteByIndexes", ...selectedIndexes.value);
  selectedIndexes.value.length = 0;
};

const deleteByIndex = (index) => {
  const idx = selectedIndexes.value.indexOf(index);
  if (idx !== -1) {
    selectedIndexes.value.splice(idx, 1);
  }
  selectedIndexes.value = selectedIndexes.value.map((idx) =>
    idx > index ? idx - 1 : idx
  );

  emit("deleteByIndexes", index);
};
</script>

<template>
  <div class="w-96 py-2">
    <div class="text-center my-4">
      <button class="bg-red-500 text-white px-3 py-1" @click="batchDelete">
        Batch delete.
      </button>
    </div>
    <ul>
      <li
        class="flex justify-between border-b mb-2"
        v-for="(todo, index) in todos"
        :key="todo.name"
      >
        <label class="flex-grow py-1">
          <input type="checkbox" v-model="selectedIndexes" :value="index" />
          {{ todo.name }}
        </label>

        <div class="flex">
          <button
            class="text-white px-3 py-1"
            :class="todo.done ? 'bg-yellow-500' : 'bg-green-500'"
            @click="$emit('switchStatusByIndex', index)"
          >
            {{ todo.done ? "Undone" : "Done" }}
          </button>
          <button
            class="bg-red-500 text-white px-3 py-1"
            @click="deleteByIndex(index)"
          >
            x
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>
