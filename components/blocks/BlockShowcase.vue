<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import { useI18n } from "#imports";
import { showcaseConfig } from "~/config/showcase"; // Note: This is still imported but project.link is no longer used.
import type { ShowcaseItem } from "~/types/config/showcase";

// Internationalization setup
const { t } = useI18n();

// Twerking categories with emojis 🍑
const categories = ref([
  {
    key: "classics",
    emoji: "👑",
    description: "The foundational moves that started it all."
  },
  {
    key: "floor-work",
    emoji: "🤸‍♀️",
    description: "Gravity-defying moves, down on the dance floor."
  },
  {
    key: "hydro-twerk",
    emoji: "💧",
    description: "Slippery and fluid, perfect for pool parties."
  },
  {
    key: "global-grooves",
    emoji: "🌍",
    description: "International twerk styles from around the world."
  },
]);

// Group projects by category
const projectsByCategory = computed(() => {
  const grouped: { [key: string]: ShowcaseItem[] } = {};
  for (const item of showcaseConfig.items) {
    if (!grouped[item.type]) {
      grouped[item.type] = [];
    }
    grouped[item.type].push(item);
  }
  return grouped;
});

// Loading state
const isLoading = ref(true);
onMounted(() => {
  setTimeout(() => {
    isLoading.value = false;
  }, 500);
});

// --- MODIFICATION START ---
// This function now opens a fixed URL for all cards.
function forceSignup() {
  const signupUrl = 'https://tip.twerk.dance';
  window.open(signupUrl, '_blank', 'noopener,noreferrer');
}
// --- MODIFICATION END ---
</script>

<template>
  <section
    id="showcase"
    class="relative overflow-hidden bg-gray-900 text-white py-16 sm:py-20"
    role="region"
    aria-label="Twerk University"
  >
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <div class="mx-auto max-w-3xl text-center mb-12">
        <h2 class="text-4xl sm:text-5xl font-extrabold tracking-tight text-white">
          Twerk University 🎓
        </h2>
        <p class="mt-4 text-lg text-gray-300">
          Explore the world of twerking, one move at a time. Unlock full videos by signing up!
        </p>
      </div>

      <div v-if="!isLoading" class="space-y-12">
        <div v-for="category in categories" :key="category.key">
          <div class="px-4 sm:px-0 mb-4">
            <h3 class="text-2xl font-bold text-white flex items-center">
              <span class="text-3xl mr-3">{{ category.emoji }}</span>
              {{ category.key.replace(/-/g, ' ').replace(/\b\w/g, l => l.toUpperCase()) }}
            </h3>
            <p class="text-gray-400">{{ category.description }}</p>
          </div>

          <div class="relative">
            <div class="overflow-x-auto pb-4 -mb-4 horizontal-scroll">
              <div class="flex space-x-4 sm:space-x-6 px-4 sm:px-0">
                <div
                  v-for="(project, key) in projectsByCategory[category.key]"
                  :key="project.id || key"
                  class="flex-shrink-0 w-64 sm:w-72 md:w-80 group/card"
                  @click="forceSignup()"
                  role="button"
                  aria-label="Sign up to view content"
                >
                  <div class="bg-gray-800 rounded-lg shadow-lg overflow-hidden transition-all duration-300 hover:scale-105 hover:shadow-primary-500/30 cursor-pointer">
                    <div class="relative h-40">
                      <img
                        :src="project.image"
                        :alt="t(`showcase.items.project${key + 1}.title`)"
                        class="w-full h-full object-cover transition-transform duration-500 group-hover/card:scale-110"
                        loading="lazy"
                      />
                      <div class="absolute inset-0 bg-gradient-to-t from-black/70 via-black/20 to-transparent"></div>
                      <div class="absolute inset-0 flex items-center justify-center bg-black/30 opacity-0 group-hover/card:opacity-100 transition-opacity duration-300">
                        <svg class="w-12 h-12 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"></path></svg>
                      </div>
                    </div>
                    <div class="p-4">
                      <h4 class="text-lg font-semibold text-white truncate">
                        {{ t(`showcase.items.project${key + 1}.title`) }}
                      </h4>
                      <p class="text-gray-400 text-sm line-clamp-2 mt-1">
                        {{ t(`showcase.items.project${key + 1}.description`) }}
                      </p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-else class="space-y-12">
        <div v-for="i in 3" :key="i" class="animate-pulse">
           <div class="h-8 w-1/3 bg-gray-700 rounded-md mb-4"></div>
           <div class="flex space-x-6">
              <div v-for="j in 4" :key="j" class="flex-shrink-0 w-72 h-60 bg-gray-800 rounded-lg"></div>
           </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.horizontal-scroll {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}

.horizontal-scroll::-webkit-scrollbar {
  display: none; /* Chrome, Safari, and Opera */
}

.group\/card:hover .shadow-primary-500\/30 {
    box-shadow: 0 10px 25px -5px rgba(var(--color-primary-500), 0.3), 0 8px 10px -6px rgba(var(--color-primary-500), 0.2);
}
</style>
