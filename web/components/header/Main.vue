<template>
  <header class="bg-indigo-600">
    <div class="container mx-auto h-20 flex justify-between items-center text-white">
      <NuxtLink :to="{ name: 'index' }" class="inline-flex items-center gap-x-5">
        <SvgIcon name="icons/logo" class="w-10 h-10" />
        Task editor
      </NuxtLink>
      <NuxtLink :to="{ name: 'add' }" class="bg-white text-indigo-500 hover:opacity-80 px-4 py-2 rounded">
        Add
      </NuxtLink>
    </div>
  </header>
</template>
