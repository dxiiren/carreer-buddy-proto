<script setup lang="ts">
import { onMounted, ref, computed } from 'vue'

definePageMeta({
  layout: 'dashboard',
})

const { user, isAuthenticated, initAuth } = useAuth()
const {
  isLoading,
  progressModules,
  resumeInfo,
  overallProgress,
  loadDashboard,
} = useDashboard()
const { trackPageVisit } = useRecentActivity()

// Track which cards have animated in
const animatedCards = ref<Set<number>>(new Set())

// Show cards with staggered animation after loading
const showCard = (index: number) => {
  return !isLoading.value && animatedCards.value.has(index)
}

onMounted(async () => {
  initAuth()

  // Redirect to login if not authenticated
  if (!isAuthenticated.value) {
    navigateTo('/login')
    return
  }

  // Track page visit
  trackPageVisit('/dashboard')

  // Load dashboard data
  await loadDashboard()

  // Stagger card animations
  const cardCount = 6
  for (let i = 0; i < cardCount; i++) {
    setTimeout(() => {
      animatedCards.value.add(i)
    }, i * 100)
  }
})
</script>

<template>
  <div>
    <!-- Bento Grid Layout - Responsive -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-12 gap-4 sm:gap-5 lg:gap-6">
        <!-- Welcome Header - Full Width on all -->
        <div
          class="sm:col-span-2 lg:col-span-12 transition-all duration-500"
          :class="showCard(0) ? 'opacity-100 translate-y-0' : (isLoading ? 'opacity-100' : 'opacity-0 translate-y-4')"
        >
          <DashboardWelcomeHeader
            :user-name="user?.name || 'User'"
            :overall-progress="overallProgress"
            :loading="isLoading"
          />
        </div>

        <!-- Progress Modules - Full width on phone/tablet, 7 cols on desktop -->
        <div
          class="sm:col-span-2 lg:col-span-7 transition-all duration-500"
          :class="showCard(1) ? 'opacity-100 translate-y-0' : (isLoading ? 'opacity-100' : 'opacity-0 translate-y-4')"
        >
          <DashboardProgressModules
            :modules="progressModules"
            :loading="isLoading"
          />
        </div>

        <!-- Quick Actions - Right of Progress on desktop -->
        <div
          class="sm:col-span-2 lg:col-span-5 transition-all duration-500"
          :class="showCard(2) ? 'opacity-100 translate-y-0' : (isLoading ? 'opacity-100' : 'opacity-0 translate-y-4')"
        >
          <DashboardQuickActions :loading="isLoading" />
        </div>

        <!-- Daily Tasks -->
        <div
          class="lg:col-span-6 transition-all duration-500"
          :class="showCard(3) ? 'opacity-100 translate-y-0' : (isLoading ? 'opacity-100' : 'opacity-0 translate-y-4')"
        >
          <DashboardDailyTasks
            :loading="isLoading"
          />
        </div>

        <!-- Recent Activity -->
        <div
          class="lg:col-span-6 transition-all duration-500"
          :class="showCard(4) ? 'opacity-100 translate-y-0' : (isLoading ? 'opacity-100' : 'opacity-0 translate-y-4')"
        >
          <DashboardRecentActivity
            :loading="isLoading"
          />
        </div>

        <!-- Motivational Section - Full width -->
        <div
          class="sm:col-span-2 lg:col-span-12 transition-all duration-500"
          :class="showCard(5) ? 'opacity-100 translate-y-0' : (isLoading ? 'opacity-100' : 'opacity-0 translate-y-4')"
        >
          <DashboardMotivationalSection
            :loading="isLoading"
          />
        </div>

        <!-- Resume Preview - Full Width -->
        <div
          class="sm:col-span-2 lg:col-span-12 transition-all duration-500"
          :class="showCard(5) ? 'opacity-100 translate-y-0' : (isLoading ? 'opacity-100' : 'opacity-0 translate-y-4')"
        >
          <DashboardResumePreview
            :resume="resumeInfo"
            :loading="isLoading"
          />
        </div>
    </div>

    <!-- AI Buddy Widget -->
    <DashboardAiBuddyWidget />
  </div>
</template>

<style scoped>
/* Smooth fade transition for card animations */
.transition-all {
  transition-property: opacity, transform;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
}
</style>
