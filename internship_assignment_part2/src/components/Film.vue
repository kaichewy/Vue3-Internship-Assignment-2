<template>
    <div>
        <button @click="closeFilm">Delete</button> {{film.title}} ({{ film.year }})
        <ul>
            <li v-for="actor in displayedActors">
                <button @click="removeActor(actor)">Delete</button> {{ actor }}
            </li>
            <li v-if="!showActorForm">
                <button class="addActor" v-on:click="displayActorForm">Add Actor</button>
            </li>
            <li v-if="showActorForm">
                <div>
                    <form>
                        <label>Name: </label> 
                        <input type="text" required v-model="name" placeholder="Enter name">
                        <p v-if="showError" style="color: red;">Please enter a name</p> <!-- Error message -->
                        <div class="submit">
                            <button type="submit" @click.prevent="submitForm">Submit</button>
                            <button type="button" @click="cancelForm">Cancel</button>
                        </div>
                    </form>
                </div>
            </li>
        </ul>
    </div>
</template>


<script>
  export default {
    data() {
        return {
            showActorForm: false,
            numActors: this.film.actors.length,
            name: "",
            showError: false,
        }
    },
    props: ['film'],
    methods: {
        closeFilm() {
            this.$emit('close')
        },
        addActors() {
            this.numActors += 1
        },
        removeActor(actor) {
            const index = this.film.actors.indexOf(actor)
            this.film.actors.splice(index, 1)
            this.numActors -= 1
        },
        displayActorForm() {
            this.showActorForm = true;
        },
        cancelForm() {
            this.showActorForm = false;
            this.showError = false;
        },
        submitForm() {
            if (this.name.length === 0) {
                this.showError = true;
                return;
            }
            this.film.actors.push(this.name)
            this.showActorForm = false
            this.name = ""
            this.numActors += 1
            this.showError = false;
        }
    },
    computed: {
        displayedActors() {
            return this.film.actors.slice(0, this.numActors)
        }
    }
}
</script>



<style scoped>
    button {
        padding: 0.5rem;
        background-color: white;
        border-color: rgb(101, 141, 155);
        border-style: solid;
    }
    .addActor {
        padding: 0.5rem 3rem;
    }
    li {
        margin: 1rem 0.25rem;
    }

    .submit {
    padding: 1rem;
  }

  .submit button {
    padding: 10px 20px;
    margin-right: 10px;
    cursor: pointer;
  }
  ul {
    list-style-type: none;
}

</style>