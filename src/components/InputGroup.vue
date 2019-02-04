<template>
  <div class="input-group">
    <ValidationLabel v-for="(field, i) in fields"
                     :key="i"
                     :field-name="i"
                     :field="field"
                     v-model="model[i]"
                     :ref="i+'-field'"
                     :isMobile="isMobile"
                     @validated="fieldValidated(i, $event)"
                     :showWarnings="showWarnings"
                     :equalsTo="field.validation && field.validation.equalsTo ? [value[field.validation.equalsTo.value]] : undefined">
    </ValidationLabel>
  </div>
</template>

<script>
  import ValidationLabel from "./ValidationLabel"

  export default {
    name: "vue-input-groups",
    props: {
      fields: Object,
      value: Object,
      showWarnings: {
        type: Boolean,
        default: true
      },
      breakpoint: {
        type: Number,
        default: 576
      }
    },
    components: {
      ValidationLabel
    },
    data() {
      return {
        model: this.value,
        resultValidation: '',
        windowWidth: window.innerWidth
      }
    },
    computed: {
      isMobile() {
        return this.windowWidth <= this.breakpoint
      },
      validations() {
        let obj = {};
        for(let field in this.fields) {
          if(this.fields[field].validation) {
            obj[field] = false;
          }
        }
        return obj;
      }
    },
    watch: {
      model: {
        deep: true,
        handler() {
          this.$emit('input', this.model);
        }
      },
      resultValidation() {
        this.$emit('validated', this.resultValidation);
      }
    },
    methods: {
      validate() {
        if(Object.keys(this.validations).length>0) {
          for(let field in this.validations) {
            this.$refs[`${field}-field`][0].validate();
          }
        }
      },
      fieldValidated(field, res) {
        this.validations[field] = res;
        for(let field in this.validations) {
          if(!this.validations[field]) {
            this.resultValidation = false;
            return;
          }
        }
        this.resultValidation = true;
      }
    },
    created() {
      if(Object.keys(this.validations).length>0) {
        this.resultValidation = false;
      } else {
        this.resultValidation = true;
      }
      window.addEventListener("resize", () => {
        this.windowWidth = window.innerWidth;
      })
    }
  }
</script>

<style src="../assets/vue-multiselect.min.css"></style>
<style src="../assets/vue-tel-input.css"></style>

<style lang="sass">
</style>
