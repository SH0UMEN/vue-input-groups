<template>
    <div>
        <input-group v-model="values"
                     :fields="fields">
        </input-group>
        {{ values }}
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