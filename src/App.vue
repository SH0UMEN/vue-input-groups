<template>
    <div>
        <input-group v-model="values"
                     :fields="fields"
                     @validated="isValid=$event"
                     :show-warnings="showWarnings"
                     :breakpoint="2000">
        </input-group>
        <button @click="showWarnings=true">Show warnings</button>
        {{ showWarnings }}
        {{ values }}
        {{ isValid }}
        <pre><code class="Javascript language-Javascript">var n = 34;
</code></pre>
    </div>
</template>

<script>
    import inputGroup from './components/InputGroup'

    export default {
        name: 'app',
        components: {
            inputGroup
        },
        data() {
            return {
                isValid: '',
                showWarnings: false,
                values: {
                    passOne: "",
                    passTwo: ""
                },
                fields: {
                    passTwo: {
                        type: "password",
                        validation: {
                            required: "La la la"
                        }
                    }
                }
            }
        }
    }
</script>

<style lang="sass">
    .input-group__label
        width: 400px
        margin-left: auto
        margin-right: auto
        margin-bottom: 10px
</style>