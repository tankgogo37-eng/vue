
<script setup>
import { ref, computed } from 'vue'

const categories = ref([
  { id: 1, name: 'Electronics', description: 'Electronic products' },
  { id: 2, name: 'Clothing', description: 'Clothes and fashion' },
  { id: 3, name: 'Food', description: 'Food and beverages' },
  { id: 4, name: 'Books', description: 'Books and education' }
])

const categoryName = ref('')
const categoryDescription = ref('')
const search = ref('')
const editingId = ref(null)

// Search category
const filteredCategories = computed(() => {
  return categories.value.filter(category =>
    category.name.toLowerCase().includes(search.value.toLowerCase())
  )
})

// Add or Update category
function saveCategory() {
  if (!categoryName.value.trim()) {
    alert('Please enter category name!')
    return
  }

  if (editingId.value) {
    const category = categories.value.find(
      item => item.id === editingId.value
    )

    category.name = categoryName.value
    category.description = categoryDescription.value
  } else {
    categories.value.push({
      id: Date.now(),
      name: categoryName.value,
      description: categoryDescription.value
    })
  }

  resetForm()
}

// Edit category
function editCategory(category) {
  editingId.value = category.id
  categoryName.value = category.name
  categoryDescription.value = category.description
}

// Delete category
function deleteCategory(id) {
  if (confirm('Are you sure you want to delete this category?')) {
    categories.value = categories.value.filter(
      category => category.id !== id
    )
  }
}

// Reset form
function resetForm() {
  categoryName.value = ''
  categoryDescription.value = ''
  editingId.value = null
}
</script>

<template>
  <div class="category-container">

    <h1>Category Management</h1>
    <p class="subtitle">Manage your product categories</p>

    <!-- Category Form -->
    <div class="category-form">

      <h2>{{ editingId ? 'Edit Category' : 'Add New Category' }}</h2>

      <input
        v-model="categoryName"
        type="text"
        placeholder="Enter category name"
      />

      <input
        v-model="categoryDescription"
        type="text"
        placeholder="Enter category description"
      />

      <button @click="saveCategory" class="save-btn">
        {{ editingId ? 'Update Category' : 'Add Category' }}
      </button>

      <button
        v-if="editingId"
        @click="resetForm"
        class="cancel-btn"
      >
        Cancel
      </button>

    </div>

    <!-- Search -->
    <div class="search-box">
      <input
        v-model="search"
        type="text"
        placeholder="Search category..."
      />
    </div>

    <!-- Category Table -->
    <div class="table-container">

      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Category Name</th>
            <th>Description</th>
            <th>Action</th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="category in filteredCategories"
            :key="category.id"
          >
            <td>{{ category.id }}</td>
            <td>{{ category.name }}</td>
            <td>{{ category.description }}</td>

            <td>
              <button
                @click="editCategory(category)"
                class="edit-btn"
              >
                Edit
              </button>

              <button
                @click="deleteCategory(category.id)"
                class="delete-btn"
              >
                Delete
              </button>
            </td>
          </tr>

          <tr v-if="filteredCategories.length === 0">
            <td colspan="4">No categories found.</td>
          </tr>
        </tbody>
      </table>

    </div>

    <p class="total">
      Total Categories: {{ categories.length }}
    </p>

  </div>
</template>

<style scoped>
.category-container {
  padding: 30px;
  font-family: Arial, sans-serif;
}

h1 {
  color: #1e293b;
  margin-bottom: 5px;
}

.subtitle {
  color: #64748b;
  margin-bottom: 25px;
}

.category-form {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px #00000015;
  margin-bottom: 25px;
}

.category-form h2 {
  font-size: 20px;
  margin-bottom: 15px;
}

input {
  padding: 12px;
  width: 100%;
  margin-bottom: 12px;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  box-sizing: border-box;
}

button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 5px;
}

.save-btn {
  background: #2563eb;
  color: white;
}

.cancel-btn {
  background: #64748b;
  color: white;
}

.edit-btn {
  background: #f59e0b;
  color: white;
}

.delete-btn {
  background: #ef4444;
  color: white;
}

.search-box {
  margin-bottom: 15px;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  background: white;
}

th, td {
  padding: 15px;
  border-bottom: 1px solid #e2e8f0;
  text-align: left;
}

th {
  background: #1e293b;
  color: white;
}

tr:hover {
  background: #f8fafc;
}

.total {
  margin-top: 20px;
  font-weight: bold;
  color: #2563eb;
}
</style>
