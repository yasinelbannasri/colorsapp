<template>
    <div>
        <router-link :to="{path: '/', query: { pageNumber: index} }" v-for="(n, index) in amountOfPages" :key="index">
            <div id="pagination-entry">
                {{index}}&nbsp;
            </div>
        </router-link>
    </div>
</template>

<script>
export default {
    props: ['amountOfPages']
}
</script>

<style scoped>
    div {
        display: flex;
        justify-content: center;
    }
    router-link-exact-active {
        color: darkblue;
    }
    router-link-active {
    color:darkorange
    }

a {
    font-weight: bold;
    color: #1f7edd;
}

div {
display: inline-block;

}
#pagination-entry:hover {
    color: darkorange;
}

</style>