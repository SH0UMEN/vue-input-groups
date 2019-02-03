<template>
    <div>
        <input-group v-model="values"
                     :fields="fields"
                     @validated="isValid=$event"
                     :show-warnings="showWarnings">
        </input-group>
        <button @click="showWarnings=true">Show warnings</button>
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
                    name: ""
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