<template>
  <div :class="elementSize">
    <field-wrapper :stacked="field.stacked">
      <div :class="field.stacked ? 'pt-6 w-full' : 'py-4 md:w-1/4'">
        <slot>
          <h4 class="font-normal text-80">{{ label }}</h4>
        </slot>
      </div>
      <div class="py-4 break-words" :class="fieldClasses">
        <slot name="value">
          <p v-if="fieldValue && !shouldDisplayAsHtml" class="text-90">
            {{ fieldValue }}
          </p>
          <div
            v-else-if="fieldValue && shouldDisplayAsHtml"
            v-html="field.value"
          ></div>
          <p v-else>&mdash;</p>
        </slot>
      </div>
    </field-wrapper>
  </div>
</template>

<script>
import CopiesToClipboard from './mixins/CopiesToClipboard'

export default {
  mixins: [CopiesToClipboard],
  props: {
    index: {
      type: Number,
      required: true,
    },

    field: {
      type: Object,
      required: true,
    },

    fieldName: {
      type: String,
      default: '',
    },
  },

  mounted() {
    if (this.hasSize) {
      this.$el.parentElement.classList.add('flex-wrap')
      this.$el.parentElement.classList.add('flex')
    }
  },

  methods: {
    copy() {
      this.copyValueToClipboard(this.field.value)
    },
  },

  computed: {
    label() {
      return this.fieldName || this.field.name
    },

    fieldValue() {
      if (
        this.field.value === '' ||
        this.field.value === null ||
        this.field.value === undefined
      ) {
        return false
      }

      return String(this.field.displayedAs || this.field.value)
    },

    shouldDisplayAsHtml() {
      return this.field.asHtml
    },

    fieldClasses() {
      return this.fullWidthContent
        ? this.field.stacked
          ? 'w-full'
          : 'w-4/5'
        : this.hasSize
          ? 'w-full'
          : 'md:w-3/4'
    },

    /**
     * Return the size that should be used for the field container.
     */
    elementSize() {
      return this.field.size || 'w-full'
    },
    /**
     * Return if the field has a size
     */
    hasSize() {
      return this.field.size !== undefined;
    },
  },
}
</script>
