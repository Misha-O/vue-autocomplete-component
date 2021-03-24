<template>
    <div class="home">
        <h1>Autocomplete search</h1>
        <Autocomplete :users="users" v-if="users.length" />
        <div v-else>
            <div class="spin"></div>
            <div>Loading...</div>
        </div>
    </div>
</template>

<script>
// @ is an alias to /src
import Autocomplete from "@/components/Autocomplete";

export default {
    name: "Home",
    components: { Autocomplete },
    data() {
        return {
            users: [],
            uri: "https://jsonplaceholder.typicode.com/users",
        };
    },
    async created() {
        try {
            const response = await fetch(this.uri);
            if (!response.ok) throw Error;
            this.users = await response.json();
        } catch (error) {
            console.error(error);
        }
    },
};
</script>

<style>
.spin {
    display: block;
    width: 40px;
    height: 40px;
    margin: 30px auto;
    border: 3px solid transparent;
    border-radius: 50%;
    border-top-color: #ff8800;
    animation: spin 1s ease infinite;
}

@keyframes spin {
    to {
        -webkit-transform: rotateZ(360deg);
    }
}
</style>
