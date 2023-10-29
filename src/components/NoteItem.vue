<template>
    <div class="notes-item" 
    v-long-press="500"
    @long-press-stop="onLongPressStop">
        <h3>{{note.title}}</h3>
        <span>{{note.date}}</span>
        <p>{{note.descr}}</p>
        <button @click.prevent="showBtn = !showBtn"><img src="@/assets/img/dots.svg" alt=""></button>
        <div class="notes-item-controls" v-if="showBtn">
            <a href="#" class="btn" @click.prevent="$emit('changeNote', note.id)">
                <img src="@/assets/img/edit.svg" alt="">
                РЕДАКТИРОВАТЬ
            </a>
            <a href="#" class="btn cancel" @click.prevent="$emit('deleteNote', id)"> 
                <img src="@/assets/img/del.svg" alt="">
                УДАЛИТЬ
            </a>
            <!-- <a href="#" class="btn cancel" @click.prevent="askDel(id)"> 
                <img src="@/assets/img/del.svg" alt="">
                УДАЛИТЬ
            </a> -->
            <button @click.prevent="showBtn = false"><img src="@/assets/img/reset.svg" alt=""></button>
        </div>
    </div>
</template>

<script>
    export default {
        inject: ['searchValue'],
        props: ['note'],
        data() {
            return {
                showBtn: true,
                width: document.documentElement.clientWidth,
                showDel: false
            }
        },
        methods: {
            onLongPressStop(){
                alert('s;ldjkfs`')
            },
            getDimensions() {
                this.width = document.documentElement.clientWidth;
                console.log("sl;dkjfs;dlfjk");
                if (this.width <= 1150) {
                    this.showBtn = false
                } else {
                    this.showBtn =   true
                }
            },
            // askDel(id){
            //     this.width = document.documentElement.clientWidth;
            //     if (this.width <= 1150) {
            //         this.showDel = true
            //     } else {
            //         this.$emit('deleteNote', id)
            //     }
            // }
        },
        mounted() {
            window.addEventListener('resize', this.getDimensions);
        },
        created() {
            this.getDimensions()
        }
        }
</script>

<style lang="scss" scoped>

</style>