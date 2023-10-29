<template>
    <div class="notes">
        <div class="container">
            <div class="notes-nav">
                <h2 v-if="search.length === 0">{{ notes.length ? 'Все заметки' : 'Нет заметок' }}</h2>
                <h2 v-if="search && notes.length > 0">Поиск...</h2>
                <h2 v-if="search && notes.length === 0">Ничего не найдено...</h2>
                <button @click="grid = !grid" v-if="removable">
                    <img src="@/assets/img/grid.svg" alt="" v-if="grid">
                    <img src="@/assets/img/list.svg" alt="" v-else>
                    <span>{{ grid ? 'Сетка' : 'Список' }}</span>
                </button>
            </div>
            <transition-group tag="div" class="notes-grid" name="notes" :class="{column: grid}">
                <NoteItem 
                v-for="(note, index) in notes"
                :key="index"
                :note="note"
                @deleteNote="$emit('deleteNote', note.id)"
                @changeNote="$emit('changeNote', note.id)"
            />
            </transition-group>
        </div>
    </div>
</template>
<script>
import NoteItem from './NoteItem.vue'

export default {
  props: ['notes', 'search'],
  components: { NoteItem },
  data() {
    return {
        grid: false,
        removable: true,
    }
  },
  methods: {
    getDimensions() {
      const windowWidth = window.innerWidth;

      if (windowWidth <= 1150) {
        this.grid = true;
        this.removable = false;
      } else {
        this.grid = false;
        this.removable = true;
      }
    },
    handleWindowResize() {
      // Update the grid and removable properties when the window is resized
      this.getDimensions();
    }
  },
  mounted() {
    // Call getDimensions initially to set the initial values
    this.getDimensions();

    // Add an event listener for window resize
    window.addEventListener('resize', this.handleWindowResize);
  },
  beforeDestroy() {
    // Remove the event listener when the component is destroyed to prevent memory leaks
    window.removeEventListener('resize', this.handleWindowResize);
  }
}
</script>
<style>
    
</style>