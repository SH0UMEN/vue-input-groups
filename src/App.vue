<template>
    <div>
        <input-group v-model="values"
                     :fields="fields"
                     @validated="isValid=$event"
                     :show-warnings="showWarnings">
        </input-group>
        <input v-model="values.killMeNow" type="text">
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
                    email: "",
                    killMeNow: ""
                },
                fields: {
                    email: {
                        type: 'email',
                        validation: {
                            isEmail: "Введите корректный email",
                            required: "Поле должно быть заполнено"
                        },
                        config: {
                            warnX: "right",
                            warnY: "center"
                        }
                    },
                    password: {
                        type: 'password'
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