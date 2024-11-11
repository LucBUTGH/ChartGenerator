<template>
  <div
    class="min-h-screen bg-gradient-to-br from-indigo-50 to-purple-50 dark:from-gray-900 dark:to-indigo-950 p-8 transition-colors duration-300"
  >
    <div class="max-w-4xl mx-auto space-y-8">
      <!-- Header -->
      <div class="text-center space-y-2 relative">
        <button
          @click="toggleDarkMode"
          class="absolute right-0 top-0 p-2 rounded-lg bg-white/80 dark:bg-gray-800/80 backdrop-blur-sm shadow-lg hover:shadow-xl transition-all duration-300"
          :title="isDarkMode ? 'Switch to light mode' : 'Switch to dark mode'"
        >
          <!-- Icône Soleil pour Dark Mode -->
          <svg
            v-if="isDarkMode"
            class="w-6 h-6 text-yellow-500"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"
            />
          </svg>
          <!-- Icône Lune pour Light Mode -->
          <svg
            v-else
            class="w-6 h-6 text-gray-700"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
            />
          </svg>
        </button>
        <h1
          class="text-6xl font-bold bg-gradient-to-r from-indigo-600 to-purple-600 dark:from-indigo-400 dark:to-purple-400 text-transparent bg-clip-text"
        >
          Chart Generator
        </h1>
        <p class="text-gray-600 dark:text-gray-300">Create beautiful radar charts in seconds</p>
      </div>

      <!-- Main Form Card -->
      <div
        class="bg-white/80 dark:bg-gray-800/80 backdrop-blur-sm rounded-2xl shadow-xl p-8 transition-all duration-300 hover:shadow-2xl"
      >
        <h2 class="text-2xl font-bold text-gray-800 dark:text-white mb-6 flex items-center gap-2">
          <span>Create Chart</span>
          <svg
            class="w-6 h-6 text-indigo-500"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"
            />
          </svg>
        </h2>

        <div class="space-y-6">
          <!-- Chart Title Input -->
          <div class="group transition-all duration-200">
            <label
              for="title"
              class="block text-sm font-medium text-gray-700 dark:text-gray-200 mb-1"
              >Chart Title</label
            >
            <input
              type="text"
              id="title"
              v-model="chartTitle"
              placeholder="Enter your chart title"
              class="w-full px-4 py-3 rounded-lg border border-gray-200 dark:border-gray-700 dark:bg-gray-700 dark:text-white focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all duration-200"
            />
          </div>

          <!-- JSON Import Section -->
          <div class="space-y-2">
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-200"
              >Import JSON Data</label
            >
            <div class="flex items-center gap-4">
              <div class="relative group flex-1">
                <input
                  type="file"
                  accept=".json"
                  @change="onJsonFileChange"
                  class="absolute inset-0 w-full h-full opacity-0 cursor-pointer"
                />
                <div
                  class="w-full px-4 py-2 border border-dashed border-gray-300 rounded-lg text-gray-500 hover:border-indigo-500 hover:text-indigo-500 transition-all duration-200 flex items-center justify-center gap-2"
                >
                  <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"
                    />
                  </svg>
                  Import JSON file
                </div>
              </div>
              <button
                @click="showJsonFormat"
                class="px-4 py-2 text-indigo-600 hover:text-indigo-700 transition-colors duration-200"
              >
                View Format
              </button>
            </div>
            <p v-if="jsonError" class="text-red-500 text-sm mt-1">{{ jsonError }}</p>
          </div>

          <!-- Color Picker -->
          <div class="flex items-center gap-4">
            <label for="chartColor" class="text-sm font-medium text-gray-700 dark:text-gray-200"
              >Chart Color</label
            >
            <div class="flex items-center gap-3">
              <input
                type="color"
                id="chartColor"
                v-model="chartColor"
                class="w-12 h-12 rounded-lg cursor-pointer border-0 bg-transparent transition-transform hover:scale-110"
              />
              <div
                class="px-3 py-1 rounded-full text-sm"
                :style="{ backgroundColor: chartColor + '20' }"
              >
                {{ chartColor }}
              </div>
            </div>
          </div>

          <!-- Profile Picture Upload -->
          <div class="space-y-2">
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-200"
              >Profile Picture</label
            >
            <div class="flex items-center gap-4">
              <div class="relative group">
                <input
                  type="file"
                  id="profilePic"
                  @change="onProfilePicChange"
                  class="absolute inset-0 w-full h-full opacity-0 cursor-pointer"
                />
                <div
                  class="px-4 py-2 border border-dashed border-gray-300 rounded-lg text-gray-500 hover:border-indigo-500 hover:text-indigo-500 transition-all duration-200"
                >
                  Choose file...
                </div>
              </div>
              <div class="w-16 h-16 rounded-full overflow-hidden ring-2 ring-gray-200">
                <img
                  v-if="profilePic"
                  :src="profilePic"
                  alt="Profile"
                  class="w-full h-full object-cover transition-all duration-300 hover:scale-110"
                />
              </div>
            </div>
          </div>

          <!-- Manual Data Points Input -->
          <div class="space-y-3">
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-200"
              >Add Data Points</label
            >
            <div class="flex flex-wrap gap-3">
              <input
                type="text"
                v-model="newLabel"
                placeholder="Label"
                class="flex-1 px-4 py-2 rounded-lg border border-gray-200 dark:border-gray-700 dark:bg-gray-700 dark:text-white focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
              />
              <input
                type="number"
                v-model="newValue"
                placeholder="Value"
                class="w-32 px-4 py-2 rounded-lg border border-gray-200 dark:border-gray-700 dark:bg-gray-700 dark:text-white focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
              />
              <button
                @click="addDataPoint"
                class="px-6 py-2 bg-gradient-to-r from-green-500 to-emerald-500 text-white rounded-lg hover:from-green-600 hover:to-emerald-600 transition-all duration-200 shadow-sm hover:shadow-md"
              >
                Add
              </button>
            </div>
          </div>

          <!-- Data Points List -->
          <div v-if="dataPoints.length > 0" class="space-y-3">
            <h3 class="text-lg font-semibold text-gray-800 dark:text-white">Data Points</h3>
            <ul class="space-y-2">
              <li
                v-for="(point, index) in dataPoints"
                :key="index"
                class="flex flex-col sm:flex-row items-center justify-between px-4 py-2 bg-gray-50 dark:bg-gray-700 rounded-lg group hover:bg-gray-100 dark:hover:bg-gray-600 transition-all duration-200"
              >
                <!-- Mode affichage -->
                <template v-if="editingIndex !== index">
                  <span class="font-medium text-gray-700 dark:text-gray-200">
                    {{ point.label }}: {{ point.value }}
                  </span>
                  <div class="flex items-center gap-2">
                    <button
                      @click="startEditing(index)"
                      class="text-gray-400 hover:text-indigo-500 transition-colors duration-200"
                    >
                      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"
                        />
                      </svg>
                    </button>
                    <button
                      @click="confirmDelete(index)"
                      class="text-gray-400 hover:text-red-500 transition-colors duration-200"
                    >
                      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                        />
                      </svg>
                    </button>
                  </div>
                </template>

                <!-- Mode édition -->
                <template v-else>
                  <div class="flex flex-col sm:flex-row gap-2 sm:gap-3 items-center w-full">
                    <input
                      type="text"
                      v-model="editLabel"
                      class="flex-1 px-2 py-1 rounded border dark:border-gray-600 dark:bg-gray-800 dark:text-white"
                    />
                    <input
                      type="number"
                      v-model="editValue"
                      class="w-full sm:w-32 px-2 py-1 rounded border dark:border-gray-600 dark:bg-gray-800 dark:text-white"
                    />
                  </div>
                  <div class="flex gap-2 sm:gap-3 items-center">
                    <button
                      @click="saveEdit"
                      class="text-green-500 hover:text-green-600 transition-colors duration-200"
                    >
                      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M5 13l4 4L19 7"
                        />
                      </svg>
                    </button>
                    <button
                      @click="cancelEdit"
                      class="text-red-500 hover:text-red-600 transition-colors duration-200"
                    >
                      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M6 18L18 6M6 6l12 12"
                        />
                      </svg>
                    </button>
                  </div>
                </template>
              </li>
            </ul>
          </div>
        </div>

        <!-- Action Buttons -->
        <div class="flex gap-4 mt-8">
          <button
            @click="generateChart"
            :disabled="dataPoints.length < 3"
            class="flex-1 px-6 py-3 bg-gradient-to-r from-indigo-500 to-purple-500 text-white rounded-lg disabled:opacity-50 disabled:cursor-not-allowed hover:from-indigo-600 hover:to-purple-600 transition-all duration-200 shadow-sm hover:shadow-md"
          >
            Generate Chart
          </button>
          <button
            @click="resetForm"
            class="flex-1 px-6 py-3 bg-gray-100 text-gray-700 rounded-lg hover:bg-gray-200 transition-all duration-200"
          >
            Reset
          </button>
        </div>
      </div>

      <!-- Chart Display -->
      <div
        v-if="showChart"
        ref="chartWrapper"
        class="bg-white/80 dark:bg-gray-800/80 backdrop-blur-sm rounded-2xl shadow-xl p-8 transition-all duration-300"
      >
        <div v-if="loading" class="flex justify-center items-center h-96">
          <div
            class="animate-spin rounded-full h-12 w-12 border-4 border-indigo-500 border-t-transparent"
          ></div>
        </div>
        <div v-else class="flex items-center gap-8">
          <div class="h-96 flex-1">
            <Radar v-if="chartData" :data="chartData" :options="chartOptions" />
          </div>
          <div class="w-32 h-32 rounded-full overflow-hidden ring-4 ring-indigo-500/20">
            <img
              v-if="profilePic"
              :src="profilePic"
              alt="Profile"
              class="w-full h-full object-cover"
            />
          </div>
        </div>
        <button
          @click="downloadChart"
          class="mt-6 px-6 py-3 bg-gradient-to-r from-blue-500 to-indigo-500 text-white rounded-lg hover:from-blue-600 hover:to-indigo-600 transition-all duration-200 shadow-sm hover:shadow-md flex items-center justify-center gap-2"
        >
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"
            />
          </svg>
          Download Chart
        </button>
        <button
          @click="toggleValues"
          class="mt-6 ml-4 px-6 py-3 bg-gradient-to-r from-green-500 to-teal-500 text-white rounded-lg hover:from-green-600 hover:to-teal-600 transition-all duration-200 shadow-sm hover:shadow-md flex items-center justify-center gap-2"
        >
          <svg
            class="w-5 h-5"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
            />
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"
            />
          </svg>
          {{ showValues ? 'Hide Values' : 'Show Values' }}
        </button>
      </div>
    </div>

    <!-- JSON Format Modal -->
    <div
      v-if="showJsonModal"
      class="fixed inset-0 bg-black/50 flex items-center justify-center p-4"
    >
      <div class="bg-white dark:bg-gray-800 rounded-lg p-6 max-w-lg w-full">
        <h3 class="text-lg font-bold mb-4 text-gray-900 dark:text-white">JSON Format Example</h3>
        <pre class="bg-gray-100 dark:bg-gray-900 p-4 rounded-lg overflow-x-auto text-sm">
{
  "title": "My Chart",
  "data": [
    { "label": "Category 1", "value": 85 },
    { "label": "Category 2", "value": 90 },
    { "label": "Category 3", "value": 75 }
  ]
}</pre
        >
        <button
          @click="showJsonModal = false"
          class="mt-4 px-4 py-2 bg-gray-200 text-gray-800 rounded hover:bg-gray-300 transition-colors duration-200"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import {
  Chart as ChartJS,
  Filler,
  Legend,
  LineElement,
  PointElement,
  RadialLinearScale,
  Tooltip,
} from 'chart.js'
import html2canvas from 'html2canvas'
import { computed, ref, watchEffect } from 'vue'
import { Radar } from 'vue-chartjs'

ChartJS.register(RadialLinearScale, PointElement, LineElement, Filler, Tooltip, Legend)

export default {
  components: {
    Radar,
  },

  setup() {
    const chartTitle = ref('')
    const chartColor = ref('#4f46e5')
    const profilePic = ref(null)
    const newLabel = ref('')
    const newValue = ref('')
    const dataPoints = ref([])
    const showChart = ref(false)
    const chartWrapper = ref(null)
    const loading = ref(false)
    const isDarkMode = ref(false)
    const jsonError = ref('')
    const showJsonModal = ref(false)
    const showValues = ref(true)

    const editingIndex = ref(null)
    const editLabel = ref('')
    const editValue = ref('')

    const chartOptions = computed(() => ({
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          labels: {
            color: isDarkMode.value ? '#fff' : '#000',
          },
        },
      },
      scales: {
        r: {
          beginAtZero: true,
          suggestedMin: 0,
          suggestedMax: 100,
          angleLines: {
            color: isDarkMode.value ? '#fff' : '#333',
          },
          grid: {
            color: isDarkMode.value ? 'rgba(255, 255, 255, 0.2)' : 'rgba(0, 0, 0, 0.1)',
          },
          ticks: {
            display: showValues.value,
            stepSize: 20,
            color: isDarkMode.value ? '#fff' : '#000',
            backdropColor: 'transparent',
          },
          pointLabels: {
            color: isDarkMode.value ? '#fff' : '#000',
          },
        },
      },
    }))

    const chartData = computed(() => ({
      labels: dataPoints.value.map((point) => point.label),
      datasets: [
        {
          label: chartTitle.value || 'Data',
          data: dataPoints.value.map((point) => point.value),
          backgroundColor: chartColor.value + '26',
          borderColor: isDarkMode.value ? '#fff' : '#333',
          borderWidth: 1,
          pointBackgroundColor: chartColor.value,
          pointBorderColor: isDarkMode.value ? '#fff' : '#333',
        },
      ],
    }))

    const toggleValues = () => {
      showValues.value = !showValues.value
      if (showChart.value && chartWrapper.value) {
        const chartInstance = chartWrapper.value.$children[0]?.chartInstance
        if (chartInstance) {
          chartInstance.options.scales.r.ticks.display = showValues.value
          chartInstance.update()
        }
      }
    }

    const onJsonFileChange = (event) => {
      const file = event.target.files[0]
      if (!file) return

      const reader = new FileReader()
      reader.onload = (e) => {
        try {
          const jsonData = JSON.parse(e.target.result)

          if (!jsonData.data || !Array.isArray(jsonData.data)) {
            throw new Error('Invalid JSON format: missing data array')
          }

          dataPoints.value = []
          jsonError.value = ''

          if (jsonData.title) {
            chartTitle.value = jsonData.title
          }

          jsonData.data.forEach((point) => {
            if (typeof point.label === 'string' && typeof point.value === 'number') {
              dataPoints.value.push({
                label: point.label,
                value: point.value,
              })
            }
          })
        } catch (error) {
          jsonError.value = `Error parsing JSON: ${error.message}`
        }
      }
      reader.readAsText(file)
    }

    const showJsonFormat = () => {
      showJsonModal.value = true
    }

    const startEditing = (index) => {
      editingIndex.value = index
      editLabel.value = dataPoints.value[index].label
      editValue.value = dataPoints.value[index].value
    }

    const saveEdit = () => {
      if (editingIndex.value !== null && editLabel.value.trim() && editValue.value) {
        dataPoints.value[editingIndex.value] = {
          label: editLabel.value.trim(),
          value: parseFloat(editValue.value),
        }
        cancelEdit()
      }
    }

    const cancelEdit = () => {
      editingIndex.value = null
      editLabel.value = ''
      editValue.value = ''
    }

    const addDataPoint = () => {
      if (newLabel.value.trim() && newValue.value) {
        dataPoints.value.push({
          label: newLabel.value.trim(),
          value: parseFloat(newValue.value),
        })
        newLabel.value = ''
        newValue.value = ''
      }
    }

    const removeDataPoint = (index) => {
      dataPoints.value.splice(index, 1)
      if (editingIndex.value === index) {
        cancelEdit()
      }
    }

    const confirmDelete = (index) => {
      if (confirm('Are you sure you want to delete this data point?')) {
        removeDataPoint(index)
      }
    }

    const generateChart = () => {
      if (dataPoints.value.length >= 3) {
        loading.value = true
        setTimeout(() => {
          showChart.value = true
          loading.value = false
        }, 1000)
      }
    }

    const downloadChart = () => {
      if (chartWrapper.value) {
        //Get all buttons in the chartWrapper
        const buttons = chartWrapper.value.querySelectorAll('button')

        if (buttons) {
          buttons.forEach((element) => {
            element.style.display = 'none'
          })
        }

        setTimeout(() => {
          html2canvas(chartWrapper.value, { useCORS: true, scale: 2 }).then((canvas) => {
            const link = document.createElement('a')
            link.download = `${chartTitle.value || 'Radar Chart'}.png`
            link.href = canvas.toDataURL('image/png')
            link.click()

            buttons.forEach((element) => {
              element.style.display = 'flex'
            })
          })
        }, 0)
      }
    }

    const onProfilePicChange = (e) => {
      const file = e.target.files[0]
      if (file) {
        profilePic.value = URL.createObjectURL(file)
      }
    }

    const resetForm = () => {
      chartTitle.value = ''
      chartColor.value = '#4f46e5'
      profilePic.value = null
      newLabel.value = ''
      newValue.value = ''
      dataPoints.value = []
      showChart.value = false
      jsonError.value = ''
    }

    const toggleDarkMode = () => {
      isDarkMode.value = !isDarkMode.value
      if (isDarkMode.value) {
        document.documentElement.classList.add('dark')
      } else {
        document.documentElement.classList.remove('dark')
      }
    }

    watchEffect(() => {
      if (showChart.value && chartWrapper.value) {
        const chartInstance = chartWrapper.value.$children[0]?.chartInstance
        if (chartInstance) {
          chartInstance.options.scales.r.angleLines.color = isDarkMode.value ? '#fff' : '#333'
          chartInstance.options.scales.r.grid.color = isDarkMode.value
            ? 'rgba(255, 255, 255, 0.2)'
            : 'rgba(0, 0, 0, 0.1)'
          chartInstance.update()
        }
      }
    })

    return {
      chartTitle,
      chartColor,
      profilePic,
      newLabel,
      newValue,
      dataPoints,
      showChart,
      chartData,
      chartOptions,
      showValues,
      toggleValues,
      chartWrapper,
      loading,
      editingIndex,
      editLabel,
      editValue,
      jsonError,
      showJsonModal,
      startEditing,
      saveEdit,
      cancelEdit,
      addDataPoint,
      removeDataPoint,
      confirmDelete,
      generateChart,
      downloadChart,
      onProfilePicChange,
      resetForm,
      toggleDarkMode,
      isDarkMode,
      onJsonFileChange,
      showJsonFormat,
    }
  },
}
</script>
