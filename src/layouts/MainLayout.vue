<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
      </q-toolbar>
      <div class="q-px-lg q-pt-xl q-mb-md">
        <div class="text-h3">To-Do</div>
        <div class="text-subtitle1">{{ getCurrentDate }}</div>
      </div>
      <q-img
        src="src/assets/statics/silver_and_golden.jpg"
        class="header-image absolute-top"
      />
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="200"
      :breakpoint="600"
    >
      <q-scroll-area style="height: calc(100% - 192px); margin-top: 192px; border-right: 1px solid #ddd">
        <q-list padding>
          <q-item clickable v-ripple exact to="/">
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section>
              To-Do
            </q-item-section>
          </q-item>

          <q-item clickable v-ripple exact to="/meet-team">
            <q-item-section avatar>
              <q-icon name="group" />
            </q-item-section>

            <q-item-section>
              Meet the Team
            </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <q-img class="absolute-top" src="src/assets/statics/silver_and_golden_sunlight.png" style="height: 192px">
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="src/assets/statics/silver_and_golden_medieval_warriors.png">
          </q-avatar>
          <div class="text-weight-bold">Silver & Golden</div>
          <div>@silverandgolden</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <router-view v-slot="{ Component }">
        <keep-alive>
          <component :is="Component" />
        </keep-alive>
      </router-view>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref, computed } from 'vue'
import { date } from 'quasar'

export default defineComponent({
  name: 'MainLayout',

  components: {
  },

  setup () {
    const leftDrawerOpen = ref(false)

    const getCurrentDate = computed(() => {
      return date.formatDate(Date.now(), 'dddd D MMMM ')
    })

    return {
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      },
      getCurrentDate
    }
  }
})
</script>

<style lang="scss">
  .header-image {
    height: 100%;
    z-index: -1;
    opacity: 0.5;
    filter: grayscale(100%);
  }
</style>
