<template>
  <div>
    <!-- @slot Use this slot header -->
    <slot name="header"></slot>

    <table class="grid">
      <!-- -->
    </table>

    <!-- @slot Use this slot footer -->
    <slot name="footer"></slot>
  </div>
</template>

<script>
/**
 * @vuepress
 * ---
 * title: Test component
 * headline: The doc headline
 * sidebarDepth: 2
 * ---
 */

import { text } from './utils';

/**
 * This is an example of creating a reusable grid component and using it with external data.
 * @version 1.0.5
 * @author [Rafael](https://github.com/rafaesc92)
 * @since Version 1.0.1
 */
export default {
  name: 'grid',
  props: {

    /**
     * object/array defaults should be returned from a factory function
     * @version 1.0.5
     * @since Version 1.0.1
     * @see See [Wikipedia](https://en.wikipedia.org/wiki/Web_colors#HTML_color_names) for a list of color names
     * @link See [Wikipedia](https://en.wikipedia.org/wiki/Web_colors#HTML_color_names) for a list of color names
     */
    msg: {
      type: [String, Number],
      default: text,
    },
    /**
     * Function
     */
    myF: {
      type: Function,
      default: (param, param2) => {}
    },
    /**
     * Model example
     * @model
     */
    value: {
      type: String
    },
    /**
     * describe data
     * @version 1.0.5
     */
    data: [Array],
    /**
     * get columns list
     */
    columns: [Array],
    /**
     * filter key
     * @ignore
     */
    filterKey: {
      type: String,
      default: 'example'
    }
  },
  data () {
    var sortOrders = {}
    this.columns.forEach(function (key) {
      sortOrders[key] = 1
    })
    return {
      sortKey: '',
      sortOrders: sortOrders
    }
  },
  computed: {
    filteredData: function () {
      var sortKey = this.sortKey
      var filterKey = this.filterKey && this.filterKey.toLowerCase()
      var order = this.sortOrders[sortKey] || 1
      var data = this.data
      if (filterKey) {
        data = data.filter(function (row) {
          return Object.keys(row).some(function (key) {
            return String(row[key]).toLowerCase().indexOf(filterKey) > -1
          })
        })
      }
      if (sortKey) {
        data = data.slice().sort(function (a, b) {
          a = a[sortKey]
          b = b[sortKey]
          return (a === b ? 0 : a > b ? 1 : -1) * order
        })
      }
      return data
    }
  },
  filters: {
    capitalize: function (str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  },
  methods: {

    /**
     * Sets the order
     *
     * @public
     * @version 1.0.5
     * @since Version 1.0.1
     * @param {string} key Key to order
     * @returns {string} Test
     */
    sortBy: function (key) {
      this.sortKey = key
      this.sortOrders[key] = this.sortOrders[key] * -1;

      /**
       * Success event.
       *
       * @event success
       * @property {object} example the demo example
       * @property {string} exampleStr the demo example
       * @property {number} exampleNum the demo example
       * @type {object | string}
       */
      this.$emit('success', {
        demo: 'example',
      })
    },
    /**
     * This is another public documented method
     * @public
     * @category Awesome methods
     * @param {*} a - First argument
     * @param {Object[]|CustomType} b - Second argument
     * @param {...*} args - Third argument
     * @return {Object}
     */
    otherPublicMethod: function (a, b, ...args) {
      return { a, b, ...args }
    },

    hiddenMethod: function(){

    }
  }
}
</script>
