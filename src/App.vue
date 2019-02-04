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
                }
            }
        }
    }
</script>

<style lang="sass">
    .validation-label
        width: 200px
        height: 50px
        margin-left: auto
        margin-right: auto
</style>