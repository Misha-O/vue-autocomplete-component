<template>
    <div class="autocomplete">
        <div class="form__group field">
            <input
                type="input"
                class="form__field"
                name="name"
                autocomplete="off"
                v-model="search"
                @input="handleInput"
                @keydown.down="onArrowDown"
                @keydown.up="onArrowUp"
                @keydown.enter="onEnter"
                placeholder="Start typing a name..."
            />
            <label for="name" class="form__label">Search</label>
        </div>
        <ul v-show="isVisible" class="autocomplete__list">
            <li v-for="(user, index) in filteredResults" :key="user.id" class="autocomplete__item" :class="{ isActive: index === counter }" @click="setResult(user.name)">
                <p>{{ user.name }}</p>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: "Autocomplete",
    props: {
        users: {
            type: Array,
            required: true,
        },
    },
    data() {
        return {
            search: "",
            results: [],
            filteredResults: [],
            isVisible: false,
            counter: -1,
            loading: false,
        };
    },
    methods: {
        filterSearch() {
            this.filteredResults = this.users.filter(user => {
                return user.name.toLowerCase().startsWith(this.search.toLowerCase());
            });
        },
        handleInput() {
            this.filterSearch();
            this.isVisible = true;
        },
        setResult(user) {
            this.search = user;
            this.isVisible = false;
        },
        onArrowDown() {
            if (this.counter < this.filteredResults.length) {
                this.counter = this.counter + 1;
            }
        },
        onArrowUp() {
            if (this.counter > 0) {
                this.counter = this.counter - 1;
            }
        },
        onEnter() {
            this.search = this.filteredResults[this.counter].name;
            this.arrowCounter = -1;
            this.isVisible = false;
        },
    },
};
</script>

<style>
.autocomplete {
    justify-content: center;
    display: flex;
    flex-direction: column;
    align-items: center;
}
/* input */
.form__group {
    position: relative;
    padding: 15px 0 0;
    margin-top: 10px;
    width: 50%;
}

.form__field {
    font-family: inherit;
    width: 100%;
    border: 0;
    border-bottom: 2px solid #9b9b9b;
    outline: 0;
    font-size: 1.3rem;
    padding: 7px 0;
    background: transparent;
    transition: border-color 0.2s;
}
.form__field::placeholder {
    color: transparent;
}

.form__field:placeholder-shown ~ .form__label {
    font-size: 1.3rem;
    cursor: text;
    top: 20px;
}

.form__label {
    position: absolute;
    top: 0;
    display: block;
    transition: 0.2s;
    font-size: 1rem;
    color: #9b9b9b;
}

.form__field:focus {
    padding-bottom: 6px;
    font-weight: 700;
    border-width: 3px;
    border-image: linear-gradient(to right, #11998e, #38ef7d);
    border-image-slice: 1;
}
.form__field:focus .form__label {
    position: absolute;
    top: 0;
    display: block;
    transition: 0.2s;
    font-size: 1rem;
    color: #11998e;
    font-weight: 700;
}
/* results list */
.autocomplete__list {
    width: 50%;
    padding: 0;
    margin: 0;
    border: 2px solid #eeeeee;
    height: 160px;
    min-height: 3em;
    max-height: 10em;
    overflow: auto;
}
.autocomplete__item {
    list-style: none;
    text-align: center;
    padding: 2px 2px;
    cursor: pointer;
}
.autocomplete__item:hover {
    background-color: #4aae9b;
    color: white;
}
.autocomplete__item.isActive,
.autocomplete__item.isActive:hover {
    background-color: #429283d7;
    color: white;
}
</style>
