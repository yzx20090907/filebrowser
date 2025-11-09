<template>
  <div
    class="context-menu__overlay"
    @click="emit('close')"
    @contextmenu.prevent="emit('close')"
  >
    <div class="context-menu" :style="menuStyle" @click.stop>
      <button
        v-for="action in actions"
        :key="action.id"
        type="button"
        class="context-menu__item"
        @click="select(action.id)"
      >
        <i class="material-icons">{{ action.icon }}</i>
        <span>{{ action.label }}</span>
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted } from "vue";

const props = defineProps<{
  x: number;
  y: number;
  actions: { id: string; label: string; icon: string }[];
}>();

const emit = defineEmits<{
  (e: "select", value: string): void;
  (e: "close"): void;
}>();

const menuStyle = computed(() => ({
  top: `${props.y}px`,
  left: `${props.x}px`,
}));

const select = (id: string) => {
  emit("select", id);
};

const handleKey = (event: KeyboardEvent) => {
  if (event.key === "Escape") {
    emit("close");
  }
};

onMounted(() => {
  document.addEventListener("keydown", handleKey);
});

onBeforeUnmount(() => {
  document.removeEventListener("keydown", handleKey);
});
</script>

<style scoped>
.context-menu__overlay {
  position: fixed;
  inset: 0;
  z-index: 2000;
  cursor: default;
}

.context-menu {
  position: absolute;
  min-width: 220px;
  padding: 0.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
  background: rgba(255, 255, 255, 0.92);
  border-radius: 1.2rem;
  box-shadow: 0 28px 48px rgba(90, 167, 255, 0.22);
  border: 1px solid rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(20px);
}

.context-menu__item {
  border: 0;
  background: transparent;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.65rem 0.85rem;
  color: var(--textSecondary);
  font-size: 0.95rem;
  border-radius: 0.85rem;
  cursor: pointer;
  transition: 0.18s ease all;
}

.context-menu__item i {
  font-size: 1.2rem;
  color: rgba(90, 167, 255, 0.9);
}

.context-menu__item:hover {
  background: rgba(90, 167, 255, 0.14);
  color: var(--dark-blue);
}

.context-menu__item:hover i {
  color: var(--dark-blue);
}
</style>
