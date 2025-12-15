<template>
  <div class="server-status-page">
    <!-- Background Elements -->
    <div class="absolute inset-0 overflow-hidden">
      <div class="absolute -top-40 -right-40 w-80 h-80 bg-purple-500 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-pulse"></div>
      <div class="absolute -bottom-40 -left-40 w-80 h-80 bg-blue-500 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-pulse"></div>
    </div>

    <!-- Navigation -->
    <nav class="relative z-10 mb-8">
      <div class="max-w-6xl mx-auto flex justify-between items-center">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 bg-gradient-to-r from-blue-500 to-purple-600 rounded-xl flex items-center justify-center">
            <Server class="w-6 h-6 text-white" />
          </div>
          <h1 class="text-2xl font-bold gradient-text" v-if="config.value?.frontend?.web_server_prefix !== undefined">{{ config.value.frontend.web_server_prefix }}</h1>
        </div>

        <div class="flex items-center space-x-4">
          <button
            @click="logout"
            class="glass-button text-white hover:text-red-300 transition-colors duration-300 flex items-center space-x-2"
          >
            <LogOut class="w-4 h-4" />
            <span>{{ $t('nav.logout') }}</span>
          </button>
          <LanguageSwitcher />
        </div>
      </div>
    </nav>

    <!-- Main Content -->
    <main class="relative z-10 max-w-6xl mx-auto">
      <div class="glass-card p-8 animate-scale-in">
        <!-- Header -->
        <div class="text-center mb-8">
          <div class="w-16 h-16 bg-gradient-to-r from-blue-500 to-purple-600 rounded-2xl flex items-center justify-center mx-auto mb-4">
            <Activity class="w-8 h-8 text-white" />
          </div>
          <h1 class="text-3xl font-bold gradient-text mb-2">
            {{ $t('server_status.title') }}
          </h1>
          <p class="text-gray-300">
            {{ $t('server_status.subtitle') }}
          </p>
        </div>

        <!-- Status Grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-8">
          <!-- Players -->
          <div class="status-card">
            <div class="flex items-center space-x-3 mb-4">
              <div class="w-12 h-12 bg-gradient-to-r from-green-500 to-emerald-600 rounded-xl flex items-center justify-center">
                <Users class="w-6 h-6 text-white" />
              </div>
              <div>
                <h3 class="text-xl font-semibold text-white">{{ $t('server_status.players.title') }}</h3>
                <p class="text-gray-400 text-sm">{{ $t('server_status.players.subtitle') }}</p>
              </div>
            </div>
            <div class="text-4xl font-bold gradient-text">
              {{ playerCount !== null ? playerCount : '--' }}
            </div>
          </div>

          <!-- Plugins -->
          <div class="status-card">
            <div class="flex items-center space-x-3 mb-4">
              <div class="w-12 h-12 bg-gradient-to-r from-orange-500 to-red-600 rounded-xl flex items-center justify-center">
                <Puzzle class="w-6 h-6 text-white" />
              </div>
              <div>
                <h3 class="text-xl font-semibold text-white">{{ $t('server_status.plugins.title') }}</h3>
                <p class="text-gray-400 text-sm">{{ $t('server_status.plugins.subtitle') }}</p>
              </div>
            </div>
            <div class="text-4xl font-bold gradient-text">
              {{ pluginCount !== null ? pluginCount : '--' }}
            </div>
          </div>

          <!-- Uptime -->
          <div class="status-card">
            <div class="flex items-center space-x-3 mb-4">
              <div class="w-12 h-12 bg-gradient-to-r from-cyan-500 to-blue-600 rounded-xl flex items-center justify-center">
                <Clock class="w-6 h-6 text-white" />
              </div>
              <div>
                <h3 class="text-xl font-semibold text-white">{{ $t('server_status.uptime.title') }}</h3>
                <p class="text-gray-400 text-sm">{{ $t('server_status.uptime.subtitle') }}</p>
              </div>
            </div>
            <div class="text-2xl font-bold gradient-text">
              {{ uptime !== null ? uptime : '--' }}
            </div>
          </div>
        </div>

        <!-- Refresh Button -->
        <div class="text-center">
          <button
            @click="refreshStatus"
            :disabled="loading"
            class="glass-button text-white hover:text-blue-300 transition-colors duration-300 flex items-center space-x-2 mx-auto px-6 py-3"
          >
            <RefreshCw :class="{ 'animate-spin': loading }" class="w-5 h-5" />
            <span>{{ loading ? $t('common.loading') : $t('server_status.refresh') }}</span>
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, inject } from 'vue'
import { useRouter } from 'vue-router'
import { Server, Users, Puzzle, Clock, Activity, RefreshCw, LogOut } from 'lucide-vue-next'
import LanguageSwitcher from '@/components/LanguageSwitcher.vue'

const config = inject('config', { value: {} as any })
const router = useRouter()

// 响应式数据
const playerCount = ref<number | null>(null)
const pluginCount = ref<number | null>(null)
const uptime = ref<string | null>(null)
const loading = ref(false)

// 获取服务器状态数据
const fetchServerStatus = async () => {
  loading.value = true
  try {
    // TODO: 实现获取服务器状态的API调用
    // const response = await fetch('/api/server/status')
    // const data = await response.json()
    // playerCount.value = data.players
    // pluginCount.value = data.plugins
    // uptime.value = data.uptime

    // 临时模拟数据
    await new Promise(resolve => setTimeout(resolve, 1000))
    playerCount.value = Math.floor(Math.random() * 100) + 10
    pluginCount.value = Math.floor(Math.random() * 20) + 5
    uptime.value = '2d 14h 32m'
  } catch (error) {
    console.error('Failed to fetch server status:', error)
    playerCount.value = null
    pluginCount.value = null
    uptime.value = null
  } finally {
    loading.value = false
  }
}

// 刷新状态
const refreshStatus = () => {
  fetchServerStatus()
}

// 退出登录
const logout = () => {
  localStorage.removeItem('admin_token')
  router.push('/login')
}

// 组件挂载时获取数据
onMounted(() => {
  fetchServerStatus()
})
</script>

<style scoped>
.server-status-page {
  min-height: 100vh;
  padding: 2rem;
  position: relative;
  overflow: hidden;
  background: #000000;
}

.glass-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 24px;
  box-shadow: 0 20px 40px -10px rgba(0, 0, 0, 0.3);
}

.status-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  padding: 24px;
  transition: all 0.3s ease;
}

.status-card:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
}

.gradient-text {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.glass-button {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.glass-button:hover:not(:disabled) {
  background: rgba(255, 255, 255, 0.15);
  border-color: rgba(255, 255, 255, 0.3);
  transform: translateY(-1px);
}

.glass-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.animate-scale-in {
  animation: scaleIn 0.6s ease-out;
}

@keyframes scaleIn {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}
</style>