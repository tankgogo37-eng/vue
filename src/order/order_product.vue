
<script setup>
import { ref, computed } from 'vue'

const orders = ref([
  {
    id: 1001,
    customer: 'John',
    product: 'Laptop',
    quantity: 1,
    price: 800,
    status: 'Pending'
  },
  {
    id: 1002,
    customer: 'David',
    product: 'Phone',
    quantity: 2,
    price: 300,
    status: 'Completed'
  },
  {
    id: 1003,
    customer: 'Sarah',
    product: 'Keyboard',
    quantity: 1,
    price: 50,
    status: 'Processing'
  }
])

const customer = ref('')
const product = ref('')
const quantity = ref(1)
const price = ref(0)
const status = ref('Pending')

const search = ref('')
const editingId = ref(null)

// Search orders
const filteredOrders = computed(() => {
  return orders.value.filter(order =>
    order.customer.toLowerCase().includes(search.value.toLowerCase()) ||
    String(order.id).includes(search.value)
  )
})

// Calculate total
const totalAmount = computed(() => {
  return orders.value.reduce(
    (total, order) => total + order.quantity * order.price,
    0
  )
})

// Add or Update Order
function saveOrder() {
  if (
    !customer.value.trim() ||
    !product.value.trim() ||
    quantity.value < 1 ||
    price.value <= 0
  ) {
    alert('Please enter valid order information!')
    return
  }

  const orderData = {
    customer: customer.value,
    product: product.value,
    quantity: Number(quantity.value),
    price: Number(price.value),
    status: status.value
  }

  if (editingId.value) {
    const index = orders.value.findIndex(
      order => order.id === editingId.value
    )

    if (index !== -1) {
      orders.value[index] = {
        ...orders.value[index],
        ...orderData
      }
    }
  } else {
    orders.value.push({
      id: Date.now(),
      ...orderData
    })
  }

  resetForm()
}

// Edit Order
function editOrder(order) {
  editingId.value = order.id
  customer.value = order.customer
  product.value = order.product
  quantity.value = order.quantity
  price.value = order.price
  status.value = order.status
}

// Delete Order
function deleteOrder(id) {
  if (confirm('Are you sure you want to delete this order?')) {
    orders.value = orders.value.filter(
      order => order.id !== id
    )
  }
}

// Reset Form
function resetForm() {
  customer.value = ''
  product.value = ''
  quantity.value = 1
  price.value = 0
  status.value = 'Pending'
  editingId.value = null
}
</script>

<template>
  <div class="order-container">

    <!-- Header -->
    <div class="header">
      <div>
        <h1>Order Management</h1>
        <p>Manage your customer orders</p>
      </div>
    </div>

    <!-- Statistics -->
    <div class="stats">

      <div class="stat-card">
        <h3>Total Orders</h3>
        <h2>{{ orders.length }}</h2>
      </div>

      <div class="stat-card">
        <h3>Total Revenue</h3>
        <h2>${{ totalAmount.toFixed(2) }}</h2>
      </div>

      <div class="stat-card">
        <h3>Pending Orders</h3>
        <h2>
          {{ orders.filter(o => o.status === 'Pending').length }}
        </h2>
      </div>

    </div>

    <!-- Order Form -->
    <div class="order-form">

      <h2>{{ editingId ? 'Edit Order' : 'Create New Order' }}</h2>

      <div class="form-grid">

        <div>
          <label>Customer Name</label>
          <input
            v-model="customer"
            placeholder="Enter customer name"
          />
        </div>

        <div>
          <label>Product Name</label>
          <input
            v-model="product"
            placeholder="Enter product name"
          />
        </div>

        <div>
          <label>Quantity</label>
          <input
            v-model.number="quantity"
            type="number"
            min="1"
          />
        </div>

        <div>
          <label>Price ($)</label>
          <input
            v-model.number="price"
            type="number"
            min="0"
            step="0.01"
          />
        </div>

        <div>
          <label>Order Status</label>
          <select v-model="status">
            <option>Pending</option>
            <option>Processing</option>
            <option>Completed</option>
            <option>Cancelled</option>
          </select>
        </div>

      </div>

      <div class="form-actions">
        <button class="add-btn" @click="saveOrder">
          {{ editingId ? 'Update Order' : '+ Add Order' }}
        </button>

        <button
          v-if="editingId"
          class="cancel-btn"
          @click="resetForm"
        >
          Cancel
        </button>
      </div>

    </div>

    <!-- Search -->
    <div class="search">
      <input
        v-model="search"
        placeholder="Search by customer or order ID..."
      />
    </div>

    <!-- Order Table -->
    <div class="table-wrapper">

      <table>
        <thead>
          <tr>
            <th>Order ID</th>
            <th>Customer</th>
            <th>Product</th>
            <th>Quantity</th>
            <th>Price</th>
            <th>Total</th>
            <th>Status</th>
            <th>Action</th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="order in filteredOrders"
            :key="order.id"
          >
            <td>#{{ order.id }}</td>
            <td>{{ order.customer }}</td>
            <td>{{ order.product }}</td>
            <td>{{ order.quantity }}</td>
            <td>${{ order.price.toFixed(2) }}</td>
            <td>
              ${{ (order.quantity * order.price).toFixed(2) }}
            </td>

            <td>
              <span
                class="status"
                :class="order.status.toLowerCase()"
              >
                {{ order.status }}
              </span>
            </td>

            <td class="actions">
              <button
                class="edit-btn"
                @click="editOrder(order)"
              >
                Edit
              </button>

              <button
                class="delete-btn"
                @click="deleteOrder(order.id)"
              >
                Delete
              </button>
            </td>
          </tr>

          <tr v-if="filteredOrders.length === 0">
            <td colspan="8" class="empty">
              No orders found.
            </td>
          </tr>
        </tbody>
      </table>

    </div>

    <div class="footer">
      Showing {{ filteredOrders.length }} of {{ orders.length }} orders
    </div>

  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

.order-container {
  padding: 30px;
  background: #f8fafc;
  min-height: 100vh;
  font-family: Arial, sans-serif;
  color: #1e293b;
}

.header h1 {
  margin-bottom: 5px;
}

.header p {
  color: #64748b;
  margin-bottom: 25px;
}

/* Statistics */
.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-bottom: 25px;
}

.stat-card {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px #00000010;
}

.stat-card h3 {
  color: #64748b;
  font-size: 14px;
}

.stat-card h2 {
  color: #2563eb;
}

/* Form */
.order-form {
  background: white;
  padding: 25px;
  border-radius: 10px;
  margin-bottom: 25px;
  box-shadow: 0 2px 8px #00000010;
}

.order-form h2 {
  margin-top: 0;
  margin-bottom: 20px;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
}

.form-grid label {
  display: block;
  margin-bottom: 8px;
  font-size: 14px;
  font-weight: bold;
}

input, select {
  width: 100%;
  padding: 12px;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  outline: none;
}

input:focus, select:focus {
  border-color: #2563eb;
}

.form-actions {
  margin-top: 20px;
}

button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.add-btn {
  background: #2563eb;
  color: white;
}

.cancel-btn {
  background: #64748b;
  color: white;
  margin-left: 8px;
}

/* Search */
.search {
  margin-bottom: 15px;
}

/* Table */
.table-wrapper {
  background: white;
  border-radius: 10px;
  overflow-x: auto;
  box-shadow: 0 2px 8px #00000010;
}

table {
  width: 100%;
  border-collapse: collapse;
  min-width: 850px;
}

th, td {
  padding: 15px;
  text-align: left;
  border-bottom: 1px solid #e2e8f0;
  font-size: 13px;
}

th {
  background: #1e293b;
  color: white;
}

tr:hover {
  background: #f8fafc;
}

/* Status */
.status {
  padding: 6px 10px;
  border-radius: 20px;
  font-size: 11px;
  font-weight: bold;
}

.pending {
  background: #fef3c7;
  color: #92400e;
}

.processing {
  background: #dbeafe;
  color: #1d4ed8;
}

.completed {
  background: #dcfce7;
  color: #166534;
}

.cancelled {
  background: #fee2e2;
  color: #991b1b;
}

/* Actions */
.edit-btn {
  background: #f59e0b;
  color: white;
  margin-right: 5px;
}

.delete-btn {
  background: #ef4444;
  color: white;
}

.empty {
  text-align: center;
  color: #64748b;
}

.footer {
  margin-top: 20px;
  color: #64748b;
  font-size: 13px;
}

/* Responsive */
@media (max-width: 768px) {
  .stats {
    grid-template-columns: 1fr;
  }

  .form-grid {
    grid-template-columns: 1fr;
  }

  .order-container {
    padding: 15px;
  }
}
</style>
