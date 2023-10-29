<template>
<header class="header">
    <transition-group name="header-notes">
     <div class="header-notes" v-if="header == true"> 
        <button v-if="s">UZ</button>
        <button class="theme">THEME</button>
        <h1>Заметки</h1>
        <button @click="header = false"><img src="@/assets/img/search.svg" alt=""></button>
    </div>
    <div class="header-form" v-if="header == false">
     <img src="@/assets/img/back.svg" alt="" @click="header = true, search = ''">
        <input type="text" placeholder="Поиск..." v-model="search" required>
        <button @click="search = ''"><img src="@/assets/img/reset.svg" alt=""></button>
    </div>
    </transition-group>
   
</header>

</template>

<script>
    export default {
        data() {
            return {
                search: '',
                header: true
            }
        },
        watch: {
            search(val) {
                this.$emit('searchValue', val)
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>