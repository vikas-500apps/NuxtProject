<template>
    <div class="mx-auto bg-white rounded-lg overflow-hidden p-6">
      <div class="flex flex-col h-full">
        <div
          class="grow overflow-hidden border rounded-lg dark:border-gray-600 border-slate-900/6"
        >
          <div class="flex flex-nowrap h-full overflow-hidden">
            <div
              class="w-[400px] h-full overflow-hidden border-r border-slate-900/6"
            >
              <WebsiteMain />
            </div>
            <div class="w-[calc(100%_-_400px)] h-full overflow-hidden">
              <WebsiteRHS />
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  