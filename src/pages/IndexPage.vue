<template>
  <q-page class="q-pa-lg">
    <!-- Page Header -->
    <div class="row items-center justify-between q-mb-xl">
      <div>
        <h1 class="text-h4 text-weight-bold q-ma-none text-dark">Dashboard</h1>
        <p class="text-grey-7 q-mt-sm">Welcome back! Here's an overview of your customers.</p>
      </div>
      <q-btn
        color="primary"
        icon="add"
        label="Add New Customer"
        no-caps
        unelevated
        class="rounded-borders q-px-md q-py-sm"
        @click="showAddModal = true"
      />
    </div>

    <!-- KPI Cards -->
    <div class="row q-col-gutter-lg q-mb-xl">
      <div v-for="stat in stats" :key="stat.title" class="col-12 col-md-4">
        <q-card flat bordered class="kpi-card rounded-borders">
          <q-card-section class="row items-center no-wrap">
            <q-avatar
              :color="stat.color"
              :text-color="stat.textColor || 'white'"
              :icon="stat.icon"
              rounded
              size="48px"
            />
            <div class="q-ml-md">
              <div class="text-h5 text-weight-bolder">{{ stat.value }}</div>
              <div class="text-caption text-grey-7">{{ stat.title }}</div>
            </div>
          </q-card-section>
          <q-separator inset />
          <q-card-section class="q-py-xs">
            <div class="text-caption text-positive row items-center">
              <q-icon name="trending_up" class="q-mr-xs" />
              <span>{{ stat.trend }} from last month</span>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>

    <!-- Customer Table Section -->
    <q-card flat bordered class="rounded-borders overflow-hidden">
      <q-table flat :rows="rows" :columns="columns" row-key="id" :filter="filter" class="cms-table">
        <template v-slot:top-left>
          <div class="text-h6 text-weight-bold">Recent Customers</div>
        </template>

        <template v-slot:top-right>
          <q-input
            outlined
            dense
            debounce="300"
            v-model="filter"
            placeholder="Search customers..."
            class="bg-white"
          >
            <template v-slot:append>
              <q-icon name="search" />
            </template>
          </q-input>
        </template>

        <template v-slot:body-cell-status="props">
          <q-td :props="props">
            <q-badge
              :color="props.value === 'Active' ? 'positive' : 'grey-5'"
              rounded
              class="q-px-sm"
            >
              {{ props.value }}
            </q-badge>
          </q-td>
        </template>

        <template v-slot:body-cell-actions="props">
          <q-td :props="props">
            <q-btn flat round color="grey-7" icon="edit" size="sm" />
            <q-btn flat round color="red-7" icon="delete_outline" size="sm" />
          </q-td>
        </template>
      </q-table>
    </q-card>

    <!-- Add Customer Modal -->
    <q-dialog v-model="showAddModal" persistent>
      <q-card style="min-width: 400px" class="rounded-borders">
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6">Add New Customer</div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-separator class="q-mt-md" />

        <q-card-section class="q-gutter-md">
          <q-input
            outlined
            v-model="newCustomer.name"
            label="Full Name"
            placeholder="e.g. John Doe"
          />
          <q-input
            outlined
            v-model="newCustomer.email"
            label="Email Address"
            placeholder="e.g. john@example.com"
          />
          <q-input
            outlined
            v-model="newCustomer.phone"
            label="Phone Number"
            placeholder="e.g. +94 ..."
          />
          <q-select
            outlined
            v-model="newCustomer.type"
            :options="['Regular', 'VIP', 'Corporate']"
            label="Customer Type"
          />
        </q-card-section>

        <q-card-actions align="right" class="q-pa-md">
          <q-btn flat label="Cancel" color="dark" v-close-popup />
          <q-btn
            unelevated
            label="Save Customer"
            color="primary"
            class="rounded-borders"
            @click="addCustomer"
          />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { useQuasar } from 'quasar'

const $q = useQuasar()

const showAddModal = ref(false)
const filter = ref('')

const newCustomer = ref({
  name: '',
  email: '',
  phone: '',
  type: 'Regular',
})

const stats = [
  { title: 'Total Customers', value: '1,284', icon: 'people', color: 'dark', trend: '12%' },
  { title: 'Active Projects', value: '42', icon: 'assignment', color: 'primary', trend: '5%' },
  { title: 'Total Revenue', value: '$12,450', icon: 'payments', color: 'green-7', trend: '8%' },
]

const columns = [
  { name: 'name', align: 'left', label: 'Name', field: 'name', sortable: true },
  { name: 'email', align: 'left', label: 'Email', field: 'email', sortable: true },
  { name: 'phone', align: 'left', label: 'Phone', field: 'phone' },
  { name: 'type', align: 'center', label: 'Type', field: 'type', sortable: true },
  { name: 'status', align: 'center', label: 'Status', field: 'status', sortable: true },
  { name: 'actions', align: 'right', label: 'Actions', field: 'id' },
]

const rows = ref([
  {
    id: 1,
    name: 'Dineth Wijesinghe',
    email: 'dineth@example.com',
    phone: '+94 77 123 4567',
    type: 'VIP',
    status: 'Active',
  },
  {
    id: 2,
    name: 'Pathum Nissanka',
    email: 'pathum@example.com',
    phone: '+94 71 987 6543',
    type: 'Regular',
    status: 'Active',
  },
  {
    id: 3,
    name: 'Kasun Rajitha',
    email: 'kasun@example.com',
    phone: '+94 70 555 1122',
    type: 'Corporate',
    status: 'Inactive',
  },
  {
    id: 4,
    name: 'Wanindu Hasaranga',
    email: 'wanindu@example.com',
    phone: '+94 76 444 3322',
    type: 'VIP',
    status: 'Active',
  },
  {
    id: 5,
    name: 'Charith Asalanka',
    email: 'charith@example.com',
    phone: '+94 72 222 1111',
    type: 'Regular',
    status: 'Active',
  },
])

function addCustomer() {
  if (newCustomer.value.name && newCustomer.value.email) {
    const id = rows.value.length + 1
    rows.value.unshift({
      ...newCustomer.value,
      id,
      status: 'Active',
    })
    showAddModal.value = false
    newCustomer.value = { name: '', email: '', phone: '', type: 'Regular' }

    $q.notify({
      message: 'Customer added successfully!',
      color: 'positive',
      icon: 'check_circle',
      position: 'top-right',
    })
  } else {
    $q.notify({
      message: 'Please fill in all required fields.',
      color: 'negative',
      icon: 'warning',
      position: 'top-right',
    })
  }
}
</script>

<style lang="scss">
.kpi-card {
  transition:
    transform 0.3s ease,
    box-shadow 0.3s ease;

  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.05);
  }
}

.cms-table {
  .q-table__top {
    padding: 20px;
    background: #fafafa;
  }

  thead tr th {
    font-weight: bold;
    color: #555;
    background: #f5f5f5;
  }

  tbody tr:hover {
    background: rgba(211, 47, 47, 0.02);
  }
}

.rounded-borders {
  border-radius: 12px;
}
</style>
