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
                    name: "",
                    killMeNow: ""
                },
                fields: {
                    name: {
                        type: "text",
                        validation: {
                            required: "Это обязательное поле"
                        },
                        config: {
                            warnY: 'bottom'
                        }
                    }
                }
            }
        }
    }
</script>