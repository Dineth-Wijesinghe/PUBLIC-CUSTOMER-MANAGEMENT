<template>
  <q-layout view="lHh Lpr lFf" class="bg-grey-1">
    <!-- Header with Glassmorphism -->
    <q-header class="bg-white text-dark q-py-sm" bordered>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
          class="q-mr-sm"
        />

        <q-toolbar-title class="text-weight-bold text-h6">
          <q-icon name="dashboard_customize" color="primary" size="28px" class="q-mr-sm" />
          <span class="text-primary font-elegant-header">CMS</span> PRO
        </q-toolbar-title>

        <q-space />

        <div class="q-gutter-sm row items-center no-wrap">
          <q-btn round flat icon="notifications">
            <q-badge floating color="red" rounded />
          </q-btn>
          <q-btn round flat>
            <q-avatar size="32px">
              <img src="https://cdn.quasar.dev/img/avatar.png" />
            </q-avatar>
          </q-btn>
        </div>
      </q-toolbar>
    </q-header>

    <!-- Modern Sidebar -->
    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="260"
      :breakpoint="500"
      class="bg-dark text-white shadow-2"
    >
      <div class="column full-height">
        <!-- Logo Area -->
        <div class="q-pa-lg text-center">
          <div class="text-h5 text-weight-bolder text-white q-mb-xs">
            <span class="text-red-7">C</span>MS
          </div>
          <div class="text-caption text-grey-5">Management System</div>
        </div>

        <q-scroll-area class="col q-px-md">
          <q-list padding class="menu-list">
            <q-item
              v-for="item in menuItems"
              :key="item.title"
              clickable
              v-ripple
              :to="item.link"
              class="menu-item q-mb-sm rounded-borders"
              active-class="menu-item-active"
            >
              <q-item-section avatar>
                <q-icon :name="item.icon" size="22px" />
              </q-item-section>

              <q-item-section class="text-weight-medium">
                {{ item.title }}
              </q-item-section>
            </q-item>
          </q-list>
        </q-scroll-area>

        <!-- Footer Area -->
        <div class="q-pa-md">
          <q-btn
            flat
            no-caps
            class="full-width rounded-borders bg-red-9 text-white q-py-sm"
            label="Logout"
            icon="logout"
          />
        </div>
      </div>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from 'vue'

const leftDrawerOpen = ref(false)

const menuItems = [
  { title: 'Dashboard', icon: 'grid_view', link: '/' },
  { title: 'Customers', icon: 'people_outline', link: '/customers' },
  { title: 'Reports', icon: 'bar_chart', link: '/reports' },
  { title: 'Settings', icon: 'settings', link: '/settings' },
]

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
</script>

<style lang="scss">
.font-elegant-header {
  letter-spacing: 2px;
}

.menu-item {
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  color: #bdbdbd;

  &:hover {
    background: rgba(211, 47, 47, 0.1);
    color: white;
  }

  &-active {
    background: #d32f2f !important;
    color: white !important;
    box-shadow: 0 4px 15px rgba(211, 47, 47, 0.3);
  }
}

.rounded-borders {
  border-radius: 12px;
}
</style>
