<script setup>
import { ref } from 'vue'

const vehicles = ref([
  {
    id: 1,
    vehicle_name: 'Toyota Vios',
    vehicle_type: 'Car',
    plate_number: 'ABC-1234',
    color: 'White',
    year: 2022,
    status: 'Available'
  },
  {
    id: 2,
    vehicle_name: 'Honda Click',
    vehicle_type: 'Motorcycle',
    plate_number: 'XYZ-5678',
    color: 'Black',
    year: 2023,
    status: 'Available'
  }
])

const vehicleName = ref('')
const vehicleType = ref('')
const plateNumber = ref('')
const color = ref('')
const year = ref('')
const status = ref('Available')

const editingId = ref(null)

function addVehicle() {
  if (
    !vehicleName.value ||
    !vehicleType.value ||
    !plateNumber.value ||
    !color.value ||
    !year.value
  ) {
    alert('Please fill in all fields.')
    return
  }

  vehicles.value.push({
    id: Date.now(),
    vehicle_name: vehicleName.value,
    vehicle_type: vehicleType.value,
    plate_number: plateNumber.value,
    color: color.value,
    year: year.value,
    status: status.value
  })

  clearForm()
}

function editVehicle(vehicle) {
  editingId.value = vehicle.id

  vehicleName.value = vehicle.vehicle_name
  vehicleType.value = vehicle.vehicle_type
  plateNumber.value = vehicle.plate_number
  color.value = vehicle.color
  year.value = vehicle.year
  status.value = vehicle.status
}

function updateVehicle() {
  const vehicle = vehicles.value.find(
    (vehicle) => vehicle.id === editingId.value
  )

  if (vehicle) {
    vehicle.vehicle_name = vehicleName.value
    vehicle.vehicle_type = vehicleType.value
    vehicle.plate_number = plateNumber.value
    vehicle.color = color.value
    vehicle.year = year.value
    vehicle.status = status.value
  }

  clearForm()
}

function deleteVehicle(id) {
  if (confirm('Are you sure you want to delete this vehicle?')) {
    vehicles.value = vehicles.value.filter(
      (vehicle) => vehicle.id !== id
    )
  }
}

function clearForm() {
  vehicleName.value = ''
  vehicleType.value = ''
  plateNumber.value = ''
  color.value = ''
  year.value = ''
  status.value = 'Available'
  editingId.value = null
}
</script>

<template>
  <div class="container">
    <h1>Vehicle Management System</h1>

    <div class="form-container">
      <h2>
        {{ editingId ? 'Edit Vehicle' : 'Add Vehicle' }}
      </h2>

      <input
        v-model="vehicleName"
        type="text"
        placeholder="Vehicle Name"
      />

      <select v-model="vehicleType">
  <option value="">Select Vehicle Type</option>
  <option value="Jeepney">Jeepney</option>
  <option value="Tricycle">Tricycle</option>
  <option value="Truck">Truck</option>
  <option value="Van">Van</option>
  <option value="Car">Car</option>
  <option value="Motorcycle">Motorcycle</option>
</select>

      <input
        v-model="plateNumber"
        type="text"
        placeholder="Plate Number"
      />

      <input
        v-model="color"
        type="text"
        placeholder="Color"
      />

      <input
        v-model="year"
        type="number"
        placeholder="Year"
      />

      <select v-model="status">
        <option value="Available">Available</option>
        <option value="In Use">In Use</option>
        <option value="Maintenance">Maintenance</option>
      </select>

      <button
        v-if="!editingId"
        @click="addVehicle"
      >
        Add Vehicle
      </button>

      <button
        v-else
        @click="updateVehicle"
      >
        Update Vehicle
      </button>

      <button
        v-if="editingId"
        @click="clearForm"
        class="cancel"
      >
        Cancel
      </button>
    </div>

    <div class="table-container">
      <h2>Vehicles</h2>

      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Vehicle Name</th>
            <th>Type</th>
            <th>Plate Number</th>
            <th>Color</th>
            <th>Year</th>
            <th>Status</th>
            <th>Actions</th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="vehicle in vehicles"
            :key="vehicle.id"
          >
            <td>{{ vehicle.id }}</td>
            <td>{{ vehicle.vehicle_name }}</td>
            <td>{{ vehicle.vehicle_type }}</td>
            <td>{{ vehicle.plate_number }}</td>
            <td>{{ vehicle.color }}</td>
            <td>{{ vehicle.year }}</td>
            <td>{{ vehicle.status }}</td>

            <td>
              <button
                @click="editVehicle(vehicle)"
                class="edit"
              >
                Edit
              </button>

              <button
                @click="deleteVehicle(vehicle.id)"
                class="delete"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
