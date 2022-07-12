<template>
    <div class="back">
        <div class="container">
            <div class="d-flex justify-content-between">
                <h2>BOOLFLIX</h2>
                <div class="ms_input">
                    <input v-model="userSearch" type="text" name="search" id="search">
                    <button @click="$emit('search', userSearch)" class="btn-primary">Search</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'HeaderPart',
    data() {
        return {
            userSearch: '',
        }
    },
    methods: {
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/style.scss';
.back {
    background-color: black;
}
.container{
    padding: 1rem 0;

}
h2 {
    color: red;
    font-size: 2rem;
}
.ms_input {
    display: flex;
    padding: 5px 0;
    input {
        font-size: 1rem;
    }
    button {
        font-size: 1rem;
        text-transform: uppercase;
    }
}
</style>