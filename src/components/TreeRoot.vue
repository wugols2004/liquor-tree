<template>
  <div class="tree" role="tree">
    <ul class="tree-root">
      <node
        v-for="(node, i) in model"
        v-if="node.visible()"

        :key="node.id"
        :node="node"
        :options="options"
      />
    </ul>
  </div>
</template>

<script>
  import TreeNode from '@/components/TreeNode'
  import TreeMixin from '@/mixins/TreeMixin'
  import Tree from '@/lib/Tree'

  const defaults = {
    multiple: true,
    checkbox: false,
    checkOnSelect: false,
    autoCheckChildren: true,
    parentSelect: false,
    keyboardNavigation: true,
    paddingLeft: 24
  }

  export default {
    name: 'Tree',
    components: {
      'node': TreeNode
    },

    mixins: [TreeMixin],

    provide: _ => ({
      tree: null
    }),

    props: {
      data: {
        type: Array,
        default: _ => []
      },

      options: {
        type: Object,
        default: _ => ({})
      }
    },

    data() {
      // we should not mutating a prop directly...
      // that's why we add if it necessary
      for (let prop in defaults) {
        if ( false === (prop in this.options) ) {
          this.options[prop] = defaults[prop]
        }
      }

      return {
        model: null,
        tree: null
      }
    }
  }
</script>

<style>
  .tree {
    overflow: auto;
  }

  .tree-root,
  .tree-children {
    list-style: none;
    padding: 0;
  }

  .tree > .tree-root {
    padding: 3px;
    box-sizing: border-box;
  }
</style>
