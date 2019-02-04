<template>
    <label class="validation-label"
           :class="[field.customLabelClass, warnX, warnY, (errors.length > 0 && showWarnings) ? 'invalid' : '', isMobile ? 'mobile' : '']"
           :for="field.type=='tel' ? 'helloPupsik' : ''"
           ref="label">
      <!-- Field -->
      <div class="validation-wrapper"
           :class="field.customWrapperClass">
        <input v-if="field.type=='text' || field.type=='password' || field.type=='email' || field.type=='number'"
               :type="field.type"
               :name="fieldName"
               :placeholder="field.placeholder"
               :class="field.customInputClass"
               v-model="value"
               @input="validate();$emit('input', value)"
               @blur="validate">
        <multiselect v-else-if="field.type=='select'"
                     :options="field.options"
                     label="label"
                     trackBy="id"
                     v-model="value"
                     :placeholder="field.placeholder"
                     @input="validate();$emit('input', value)"
                     :class="field.customInputClass"
                     :multiple="config.multiple"
                     :searchable="config.searchable"
                     :clearOnSelect="config.clearOnSelect"
                     :hideSelected="config.hideSelected"
                     :closeOnSelect="config.closeOnSelect"
                     :taggable="config.taggable"
                     :tagPlaceholder="config.tagPlaceholder"
                     :tagPosition="config.tagPosition"
                     :groupValues="config.groupValues"
                     :groupLabel="config.groupLabel"
                     :groupSelect="config.groupSelect"
                     :internalSearch="config.internalSearch"
                     :preserveSearch="config.preserveSearch"
                     :preselectFirst="config.preselectFirst"
                     :name="config.name"
                     :selectLabel="config.selectLabel"
                     :selectGroupLabel="config.selectGroupLabel"
                     :selectedLabel="config.selectedLabel"
                     :deselectLabel="config.deselectLabel"
                     :deselectGroupLabel="config.deselectGroupLabel"
                     :showLabels="config.showLabels"
                     :disabled="config.disabled"
                     :maxHeight="config.maxHeight"
                     :openDirection="config.openDirection"
                     :showNoResults="config.showNoResults"
                     :tabindex="config.tabindex"
        >
        </multiselect>
        <telinput v-else-if="field.type=='tel'"
                  v-model="value"
                  @onInput="validatePhone($event);$emit('input', value)"
                  :class="field.customInputClass"
                  :defaultCountry="config.defaultCountry"
                  :disabledFetchingCountry="config.disabledFetchingCountry"
                  :disabled="config.disabled"
                  :placeholder="config.placeholder"
                  :enabledFlags="config.enabledFlags"
                  :preferredCountries="config.preferredCountries"
                  :onlyCountries="config.onlyCountries"
                  :ignoredCountries="config.ignoredCountries"
                  @onBlur="validatePhone">
        </telinput>

        <!-- Error view -->
        <transition name="error">
          <div v-if="errors.length > 0 && showWarnings" class="warning">
            <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px"
                 width="24" height="24"
                 viewBox="0 0 224 224"
                 style=" fill:#000000;">
              <g fill="none" fill-rule="nonzero" stroke="none" stroke-width="1"
                 stroke-linecap="butt" stroke-linejoin="miter" stroke-miterlimit="10"
                 stroke-dasharray="" stroke-dashoffset="0" font-family="none"
                 font-weight="none" font-size="none" text-anchor="none"
                 style="mix-blend-mode: normal">
                <path d="M0,224v-224h224v224z" fill="none"></path>
                <g fill="#ee4444">
                  <path
                    d="M210.52267,101.98533l-88.508,-88.508c-5.52533,-5.52533 -14.49467,-5.52533 -20.02,0l-88.508,88.508c-5.52533,5.52533 -5.52533,14.49467 0,20.02l88.508,88.508c5.52533,5.52533 14.49467,5.52533 20.02,0l88.508,-88.508c5.52533,-5.52533 5.52533,-14.48533 0,-20.02zM121.33333,158.66667h-18.66667v-18.66667h18.66667zM121.33333,121.33333h-18.66667v-56h18.66667z"></path>
                </g>
              </g>
            </svg>
            <div class="errors">
              <div class="errors-wrapper">
                <span v-for="error in errors" class="error">
                  {{ error }}
                </span>
              </div>
            </div>
          </div>
        </transition>
      </div>
    </label>
</template>

<script>
  import Multiselect from "vue-multiselect"
  import Telinput from 'vue-tel-input'


  export default {
    name: "ValidationLabel",
    props: {
      field: Object,
      fieldName: String,
      isMobile: {
        type: Boolean,
        default: false
      },
      showWarnings: Boolean
    },
    components: {
      Multiselect,
      Telinput
    },
    watch: {
      showWarnings() {
        if(this.showWarnings) {
          if(this.field.type == 'tel') {
            this.validatePhone();
          } else {
            this.validate();
          }
        }
      }
    },
    data() {
      return {
        errors: [],
        value: '',
        validation: this.field.validation,
        config: this.field.config || {},
        isValid: this.field.validation && (this.field.validation.required || this.field.validation.min) ? false : true,
        warnY: (this.field.config && this.field.config.warnY) ? this.field.config.warnY : 'center',
        warnX: (this.field.config && this.field.config.warnX) ? this.field.config.warnX : 'right',
      }
    },
    mounted() {
      this.$emit('validated', this.isValid);
    },
    methods: {
      validate() {
        if (this.validation) {
          //Required
          if (this.validation.hasOwnProperty('required')) {
            this.checkRule(!this.value, this.validation.required);
          }

          //Minimal length
          if (this.validation.hasOwnProperty('min')) {
            this.checkRule(this.validation.min.value > this.value.length, this.validation.min.message);
          }

          //Maximal length
          if (this.validation.hasOwnProperty('max')) {
            this.checkRule(this.validation.max.value < this.value.length, this.validation.max.message);
          }

          if (this.field.type == 'email') {
            let emailRegex = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/igm;
            this.checkRule(!emailRegex.test(this.value), this.validation ? this.validation.isEmail : 'Enter correct email');
          }

          this.emitValidation();
        }
      },
      validatePhone(e) {
        if(e) {
          this.checkRule(e.isValid==false && this.value.length>0, this.validation ? this.validation.isPhone : 'Enter correct phone number');
        }
        if(this.field.validation) {
          if(this.field.validation.required) {
            this.checkRule(this.value.length == 0, this.field.validation.required);
          }
        }
        this.emitValidation();
      },
      cutError(message) {
        this.errors.splice(this.errors.indexOf(message), 1);
      },
      pushError(message) {
        if (this.errors.indexOf(message) == -1) {
          this.errors.push(message);
        }
      },
      checkRule(condition, message) {
        if(condition) {
          this.isValid = false;
          this.pushError(message);
        } else if(this.errors.indexOf(message) != -1) {
          this.cutError(message);
        }
      },
      emitValidation() {
        if(this.errors.length == 0) {
          this.isValid = true;
        } else {
          this.isValid = false;
        }

        this.$emit('validated', this.isValid);
      }
    },
  }
</script>


<style lang="sass">
  .validation-label
    display: block
    font-size: 16px

    .validation-wrapper
      position: relative
      height: 100%

    .validation-wrapper>input, .vue-tel-input
      height: 100%
      font-size: 16px
      outline: none
      border: 1px solid #e8e8e8
      border-radius: 5px
      width: 100%
      padding: 10px 10px 10px 10px
      box-shadow: 0 0 0 0 #EE4444
      transition: box-shadow .3s
      box-sizing: border-box

      &::placeholder
        color: #adadad
    .validation-wrapper>input
      display: block
    .vue-tel-input
      outline: none !important
      ul
        z-index: 101
      &:focus-within
        box-shadow: none
        border: 1px solid #e8e8e8
      .dropdown
        padding: 0
      input
        font-size: 16px
        padding-top: 0
        padding-bottom: 0
        outline: none !important
        &::placeholder
          color: #adadad
    .multiselect
      .multiselect__placeholder
        font-size: 16px

      .multiselect__select
        display: none

      .multiselect__tags
        padding-right: 10px

      .multiselect__content-wrapper
        border: none

    .warning
      position: absolute
      top: 0
      height: 100%
      display: flex
      align-items: center
      justify-content: center

      .errors
        position: absolute
        width: 250px
        z-index: 1
        display: none
        align-items: center
        color: white
        .errors-wrapper
          display: flex
          flex-direction: column
          justify-content: center
          position: relative
          left: 0
          top: 0
          width: 100%
          background-color: #e44
          padding-bottom: 11px
          padding-top: 5px
        span
          display: block
          width: 100%
          text-align: left
          margin-top: 6px
          margin-left: 12px
          margin-right: 12px
        &:after
          display: block
          position: absolute
          width: 20px
          content: ''
          top: 50%
          margin-top: -10px
          border: 10px solid transparent
          box-sizing: content-box
      &:hover
        .errors
          display: flex

      &.error-enter-active, &.error-leave-active
        transition: opacity .3s

      &.error-enter, &.error-leave-to
        opacity: 0
    &.top
      .warning
        .errors
          bottom: 100%
          top: unset
          margin-bottom: 15px
          &:after
            width: 0px
            border: 10px solid transparent
            border-top: 10px solid #e44
            bottom: -20px
            margin-top: -0px
            top: unset
    &.bottom
      .warning
        .errors
          top: 100%
          margin-top: 15px
          &:after
            width: 0px
            border: 10px solid transparent
            border-bottom: 10px solid #e44
            top: -20px
            margin-top: 0
    &.right
      .warning
        right: 5px
      &.top
        .warning
          .errors
            right: -7px
            &:after
              right: 9px
      &.bottom
        .warning
          .errors
            right: -7px
            &:after
              right: 9px
      &.center
        .warning
          .errors
            left: 100%
            margin-left: 15px
            height: 0
            top: 50%
            &:after
              left: -40px
              border-right: 10px solid #e44
    &.left
      .warning
        left: 5px
      &.top
        .warning
          .errors
            left: -7px
            &:after
              left: 8px
      &.bottom
        .warning
          .errors
            left: -7px
            &:after
              left: 9px
      &.center
        .warning
          .errors
            height: 0
            margin-right: 15px
            right: 100%
            top: 50%
            &:after
              right: -40px
              border-left: 10px solid #e44
    &.invalid
      .validation-wrapper>input, .vue-tel-input,.multiselect .multiselect__tags
        box-shadow: 0 0 0 2px #EE4444
      &.right
        .validation-wrapper>input, .vue-tel-input, .multiselect .multiselect__tags
          padding-right: 35px
      &.left
        .validation-wrapper>input, .vue-tel-input, .multiselect .multiselect__tags
          padding-left: 35px
    &.mobile
      &.invalid.left, &.invalid.right
        .validation-wrapper>input, .multiselect .multiselect__tags, .vue-tel-input
          padding-right: 10px
          padding-left: 10px
        .warning
          .errors
            margin-left: 0
            margin-right: 0
      .warning
        position: static
        svg
          display: none
        .errors
          margin-top: 0
          margin-bottom: 0
          position: static
          display: flex
          width: 90%
          padding-top: 10px
          padding-bottom: 9px
          min-height: unset
          margin-left: auto
          margin-right: auto
          max-width: unset
          border-radius: 0 0 5px 5px
          .error
            text-align: center
          &:after
            display: none
</style>
