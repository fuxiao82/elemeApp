<template>
    <div :class="['support', {'hide-border': type === 'hideborder'}]">
      <div :class="['icon', supportType[support.type]]"></div>
      <p :class="['text',{'white': isWhite}]">
        {{support.description}}
      </p>
    </div>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'support',
    props: {
      type: {
        type: String,
        default: ''
      },
      isWhite: {
        type: Boolean,
        default: false
      },
      support: {
        type: Object
      }
    },
    data () {
      return {
        supportType: ['decrease', 'discount', 'guarantee', 'invoice', 'special']
      }
    }
  }
</script>

<style>
    @import "./support.css";
</style>
