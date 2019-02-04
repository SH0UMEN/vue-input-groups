<template>
    <div>
        <input-group v-model="values"
                     :fields="fields"
                     @validated="isValid=$event"
                     :show-warnings="showWarnings">
        </input-group>
        <button @click="showWarnings=true">Show warnings</button>
        {{ showWarnings }}
        {{ values }}
        {{ isValid }}
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
                    passOne: {
                        type: "password",
                    },
                    passTwo: {
                        type: "password",
                        validation: {
                            equalsTo: {
                                value: "passOne",
                                message: "Пароли не совпадают"
                            }
                        }
                    }
                }
            }
        }
    }
</script>

<style lang="sass">
    .validation-label
        width: 400px
        margin-left: auto
        margin-right: auto
        margin-bottom: 10px
</style>