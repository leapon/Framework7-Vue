<template>
  <div
    class="panel"
    :class="classesObject"
    :style="{'display': opened ? 'block' : ''}"
    @open="onOpen"
    @opened="onOpened"
    @close="onClose"
    @closed="onClosed"
  >
    <slot></slot>
  </div>
</template>
<script>
  export default {
    props: {
      'side': String,
      'effect': String,
      'cover': Boolean,
      'reveal': Boolean,
      'left': Boolean,
      'right': Boolean,
      'theme': String,
      'layout': String,
      'opened': Boolean
    },
    computed: {
      classesObject: function () {
        var self = this;
        var side = self.side || (self.left ? 'left' : 'right');
        var effect = self.effect || (self.reveal ? 'reveal' : 'cover');
        var co = {};
        co['panel-' + side] = true;
        co['panel-' + effect] = true;
        if (self.layout) co['layout-' + self.layout] = true;
        if (self.theme) co['theme-' + self.theme] = true;
        co['active'] = self.opened;
        return co;
      }
    },
    watch: {
      opened: function (opened) {
        var self = this;
        if (!self.$f7) return;
        var side = self.side || (self.left ? 'left' : 'right');
        if (opened) {
          self.$f7.openPanel(side);
        }
        else {
          self.$f7.closePanel(side);
        }
      }
    },
    mounted: function () {
      var self = this;
      var $$ = self.$$
      if (!$$) return;
      var side = self.side || (self.left ? 'left' : 'right');
      var effect = self.effect || (self.reveal ? 'reveal' : 'cover');
      if (self.opened) {
        $$('body').addClass('with-panel-' + side + '-' + effect)
      }
    },
    methods: {
      onOpen: function (event) {
        this.$emit('open', event);
      },
      onOpened: function (event) {
        this.$emit('opened', event);
      },
      onClose: function (event) {
        this.$emit('open', event);
      },
      onClosed: function (event) {
        this.$emit('closed', event);
      },
      onF7Init: function () {
        var $$ = this.$$
        if (!$$) return;
        if ($$('.panel-overlay').length === 0) {
          $$('<div class="panel-overlay"></div>').insertBefore(this.$el)
        }
      }
    }
  }
</script>