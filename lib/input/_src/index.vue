<template>
  <span :class='["ipt--wp", "ipt__" + theme, {
    "ipt__disabled": disabled,
    "ipt--icon__left": icon && icon.position === "left",
    "ipt--icon__right": icon && icon.position === "right"
  }]'>
    <input :class='["ipt--ipt"]' ref='input'
      @click='handleClick'
      @change='handleChange'
      @focus="handleFocus"
      @blur='handleBlur'
      @keyup='handleKeyup'
      :type='type'
      :maxlength='maxlength'
      :disabled='disabled'
      :placeholder='placeholder'
      :readOnly='readOnly'
      :value='value'/>

      <bee-icon v-if='icon' :icon='icon.icon'></bee-icon>
  </span>
</template>

<script>
import Utils from '../../utils/utils'

export default {
  name: 'BeeInput',
  props: {
    theme: {
      type: String,
      default: 'default'
    },
    type: {
      type: String,
      default: 'text'
    },
    disabled: [Boolean, String],
    placeholder: String,
    icon: Object,
    autoFocus: Boolean,
    readOnly: Boolean,
    reg: Function,
    maxlength: [Number, String],
    enterEvent: Function,
    value: String
  },
  data () {
    return {}
  },
  mounted () {
    this.$nextTick(() => {
      // auto focus
      if (this.autoFocus) {
        this.$refs.input.focus()
      }
    })
  },
  methods: {
    handleClick (evt) {
      this.$emit('click', evt)
    },

    handleChange (evt) {
      this.updateValue(evt)
      this.$emit('change', evt)
    },

    handleFocus (evt) {
      this.$emit('focus', evt)
    },

    handleBlur (evt) {
      this.$emit('blur', evt)
    },

    handleKeyup (evt) {
      this.updateValue(evt)

      // for enter key add quick entry.
      if (Utils.typeof(this.enterEvent) === 'function' && evt.keyCode === 13) {
        this.enterEvent()
      }

      this.$emit('keyup', evt)
    },

    updateValue (evt) {
      if (Utils.typeof(this.reg) === 'function') {
        evt.target.value = this.reg(evt.target.value)
      }

      this.$emit('input', evt.target.value)
    }
  }
}
</script>

<style lang="less">
@import '../../theme.less';
@root: ipt;

.@{root}--wp {
  display: inline-block;
  position: relative;
  font-size: 14px;

  .@{root}--ipt {
    height: @ipt-height;
    padding: 0 10px;
    border: 1px solid @border-color;
    border-radius: @border-radius;
    outline: none;
    box-sizing: border-box;
    width: 100%;
    font-size: inherit;

    &::placeholder {
      color: @placeholder-color;
    }
  }

  .icon--wp {
    color: @font-tint-color;
    position: absolute;
    top: 0;
    line-height: @ipt-height;
    font-size: 16px;
  }

  &.@{root}--icon__left {
    .@{root}--ipt {
      padding-left: 20px;
    }
    .icon--wp {
      left: 4px;
    }
  }

  &.@{root}--icon__right {
    .@{root}--ipt {
      padding-right: 20px;
    }
    .icon--wp {
      right: 4px;
    }
  }

  &.@{root}__primary {
    .@{root}--ipt {
      border-color: @primary-color;
    }
    .icon--wp {
      color: @primary-color;
    }
  }

  &.@{root}__success {
    .@{root}--ipt {
      border-color: @success-color;
    }
    .icon--wp {
      color: @success-color;
    }
  }

  &.@{root}__error {
    .@{root}--ipt {
      border-color: @error-color;
    }
    .icon--wp {
      color: @error-color;
    }
  }

  &.@{root}__disabled {
    opacity: .4;
    cursor: no-drop;
    .@{root}--ipt {
      cursor: no-drop;
      border-color: @border-color;
    }

    .icon--wp {
      color: @border-color;
    }
  }
}
</style>
