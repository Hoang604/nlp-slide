<script setup lang="ts">
// Clean Minimalist Architectural Backdrop (No curved lines or scribbles)
</script>

<template>
  <div class="absolute inset-0 pointer-events-none overflow-hidden select-none">
    <svg class="w-full h-full" viewBox="0 0 960 540" fill="none" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <!-- Fine Dot Grid Matrix -->
        <pattern id="minimal-dots" width="32" height="32" patternUnits="userSpaceOnUse">
          <circle cx="2" cy="2" r="0.65" class="fill-gray-400 dark:fill-gray-600" opacity="0.18" />
        </pattern>

        <!-- Soft Center Diffuse Glow -->
        <radialGradient id="center-diffuse" cx="50%" cy="45%" r="50%">
          <stop offset="0%" stop-color="#3b82f6" stop-opacity="0.05" />
          <stop offset="60%" stop-color="#8b5cf6" stop-opacity="0.02" />
          <stop offset="100%" stop-color="transparent" stop-opacity="0" />
        </radialGradient>
      </defs>

      <!-- Soft Ambient Lighting -->
      <rect width="960" height="540" fill="url(#center-diffuse)" />

      <!-- Minimal Dot Matrix Surface -->
      <rect width="960" height="540" fill="url(#minimal-dots)" />

      <!-- Clean Architectural Outer Dashed Frame -->
      <rect x="28" y="24" width="904" height="492" rx="4" 
        class="stroke-gray-200 dark:stroke-gray-800" 
        stroke-width="1" 
        stroke-dasharray="4 4" 
        opacity="0.7" />

      <!-- Precision Corner Brackets (1px crisp lines) -->
      <g class="stroke-gray-400 dark:stroke-gray-600" stroke-width="1.25">
        <!-- Top Left -->
        <path d="M 22 36 L 22 18 L 40 18" />
        <!-- Top Right -->
        <path d="M 938 36 L 938 18 L 920 18" />
        <!-- Bottom Left -->
        <path d="M 22 504 L 22 522 L 40 522" />
        <!-- Bottom Right -->
        <path d="M 938 504 L 938 522 L 920 522" />
      </g>

      <!-- Minimal Coordinate Crosshairs at Margins -->
      <g class="stroke-gray-300 dark:stroke-gray-700" stroke-width="1" opacity="0.6">
        <!-- Top Mid Crosshair -->
        <path d="M 480 12 L 480 20 M 476 16 L 484 16" />
        <!-- Bottom Mid Crosshair -->
        <path d="M 480 520 L 480 528 M 476 524 L 484 524" />
        <!-- Left Mid Crosshair -->
        <path d="M 14 270 L 22 270 M 18 266 L 18 274" />
        <!-- Right Mid Crosshair -->
        <path d="M 938 270 L 946 270 M 942 266 L 942 274" />
      </g>
    </svg>
  </div>
</template>
