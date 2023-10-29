<template>
    <Transition name="modal">
        <div class="modal" @click="$emit('closeModal')">
            <div class="modal-content" @click.stop>
                <h2>{{edit ? 'Изменить' : 'Добавить'}} заметку</h2>
                <div class="manager">
                    <input type="text" required placeholder="Title" v-model="user.title" v-if="!edit">
                    <input type="text" required placeholder="Title" v-model="editObj.title" v-else>
                    <textarea required placeholder="Content" v-model="user.descr" v-if="!edit"></textarea>
                    <textarea required placeholder="Content" v-model="editObj.descr" v-else></textarea>
                    <div class="modal-btns">
                        <a href="#" class="btn cancel dop" @click.prevent="$emit('closeModal')">ОТМЕНА</a>
                        <button @click.prevent="send" type="button" class="btn add" v-if="!edit">ДОБАВИТЬ</button>
                        <button @click="changeNote" type="button" class="btn add" v-else>ИЗМЕНИТЬ</button>
                    </div>
                </div>
            </div>
        </div>
    </Transition>
</template>

<script>
    export default {
        props: ['currentId', 'editObj', 'edit'],
        data() {
            return {
                user: {
                    title: '',
                    descr: '',
                }
            }
        },
        methods: {
            send(){
                this.$emit('addNote', {...this.user})
                this.$emit('closeModal')
                for (const key in this.user) this.user[key] = ''
                // window.location.reload()
            },
            changeNote(){
                if(this.user.title != '' && this.user.descr != ''){
                    const editNote = {
                        id: this.editObj.id,
                        title: this.user.title,
                        descr: this.user.descr,
                        date: new Date().toLocaleString()
                    }
                    this.$emit('editedNote', editNote)
                }
                this.$emit('closeModal')
                // window.location.reload()
            },
        },
    }
</script>

<style lang="scss" scoped>

</style>