<template>
  <ul>
    <component :is="resolveNavItemComponent(item)" v-for="item in items" :key="item.header || item.title" :item="item" />
  </ul>
</template>

<script>
import { resolveVerticalNavMenuItemComponent as resolveNavItemComponent } from '@core/layouts/utils'
import { provide, ref } from '@vue/composition-api'
import VerticalNavMenuHeader from '../vertical-nav-menu-header'
import VerticalNavMenuLink from '../vertical-nav-menu-link/VerticalNavMenuLink.vue'
import VerticalNavMenuGroup from '../vertical-nav-menu-group/VerticalNavMenuGroup.vue'

export default {
  components: {
    VerticalNavMenuHeader,
    VerticalNavMenuLink,
    VerticalNavMenuGroup,
  },
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  setup() {
    provide('openGroups', ref([]))

    // eslint-disable-next-line no-shadow
    const resolveNavItemComponent = item => {
      if (item.header) return 'vertical-nav-menu-header'
      if (item.children) return 'vertical-nav-menu-group'
      if (localStorage.getItem('user_role') === 'visitor' && item.title == 'Departments') return 'd-none'
      return 'vertical-nav-menu-link'

    }

    return {


      resolveNavItemComponent,
    }
  },

}
</script>
