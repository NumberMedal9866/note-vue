  <template>
  <Navbar @searchValue="search = $event" />
  <Notes 
    :notes="filterNotes"
    @deleteNote="deleteNote"
    @changeNote="changeNote"
    :search="search"
  />
  <Modal 
    v-show="showModal"
    @closeModal="showModal = false"
    @addNote="addNote"
    :currentId="currentId"
    :editObj="editObj"
    :edit="edit"
    @editedNote="editedNote"
  />
  <a href="#" class="addNote" @click.prevent="showModal = true, edit = false">
    <img src="@/assets/img/edit.svg" alt="">
  </a>
</template>
<script>
import Modal from './components/Modal.vue'

import Navbar from './components/Navbar.vue'
import Notes from './components/Notes.vue'
  export default {
    data() {
      return {
        showModal: false,
        notes: [],
        currentId: localStorage.noteId ? JSON.parse(localStorage.noteId) : 0,
        edit: false,
        editObj: {},
        search: ''
      }
    },
    methods: {
      addNote(obj){
        const note = {...obj}
        note.id = this.currentId++
        note.date = new Date().toLocaleString()
        this.notes.push(note)
        console.log(note);
      },
      getNotes(){
        const localNotes = localStorage.notes
        if (localNotes) {
          this.notes = JSON.parse(localNotes)
        }
      },
      changeNote(id){
        this.showModal = this.edit = true
        let pickedNote = this.notes.find(note => note.id === id)
        this.editObj = pickedNote
      },
      deleteNote(id){
        let index = this.notes.findIndex(note => note.id === id)
        this.notes.splice(index, 1)
        this.currentId--
        // location.reload()
      },
      editedNote(noteEdited){
        this.notes.forEach(note => {
          if(note.id == noteEdited.id){
            note.title = noteEdited.title
            note.descr = noteEdited.descr
            note.date =  noteEdited.date
          }
        })
      }
    },
    computed: {
      filterNotes(){
        return this.search ? this.notes.filter(note => note.title.toLowerCase().includes(this.search.toLowerCase())) : this.notes
      }
    },
    provide(){
      return {
        searchValue: this.search
      }
    },
    created(){
      this.getNotes()
    },
    watch: {
      notes: {
        handler(newNotes) {
          localStorage.notes = JSON.stringify(newNotes)
          localStorage.noteId = JSON.stringify(this.currentId)
        },
        deep: true
      }
    },
    components: { Navbar, Notes, Modal },
}
</script>
<style>

</style>