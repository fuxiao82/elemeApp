<template>
    <div class="support">
      <span :class="['icon', supportType[support.type]]"></span>
      <p class="text">
        {{support.description}}
      </p>
    </div>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'support',
    props: {
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
