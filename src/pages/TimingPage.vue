<template>

    <div class="sticky top-0 z-40 flex h-16 shrink-0 items-center gap-x-6 border-b border-white/5 bg-gray-900 px-4 shadow-sm sm:px-6 lg:px-8">
        <button type="button" class="-m-2.5 p-2.5 text-white xl:hidden" @click="sidebarOpen = true">
        <span class="sr-only">Open sidebar</span>
        <Bars3Icon class="h-5 w-5" aria-hidden="true" />
        </button>

        <div class="flex flex-1 gap-x-4 self-stretch lg:gap-x-6">
        <form class="flex flex-1" action="#" method="GET">
            <label for="search-field" class="sr-only">Search</label>
            <div class="relative w-full">
            <MagnifyingGlassIcon class="pointer-events-none absolute inset-y-0 left-0 h-full w-5 text-gray-500" aria-hidden="true" />
            <input id="search-field" class="block h-full w-full border-0 bg-transparent py-0 pl-8 pr-0 text-white focus:ring-0 sm:text-sm" placeholder="Search..." type="search" name="search" />
            </div>
        </form>
        </div>
    </div>

    <main class="lg:pr-96">
        <header class="flex items-center justify-between border-b border-white/5 px-4 py-4 sm:px-6 sm:py-6 lg:px-8">
        <h1 class="text-base font-semibold leading-7 text-white">TIMING LIST</h1>

        <Menu as="div" class="relative">
            <MenuButton class="flex items-center gap-x-1 text-sm font-medium leading-6 text-white">
            Sort by
            <ChevronUpDownIcon class="h-5 w-5 text-gray-500" aria-hidden="true" />
            </MenuButton>
            <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100" leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
            <MenuItems class="absolute right-0 z-10 mt-2.5 w-40 origin-top-right rounded-md bg-white py-2 shadow-lg ring-1 ring-gray-900/5 focus:outline-none">
                <MenuItem v-slot="{ active }">
                <a href="#" :class="[active ? 'bg-gray-50' : '', 'block px-3 py-1 text-sm leading-6 text-gray-900']">Name</a>
                </MenuItem>
                <MenuItem v-slot="{ active }">
                <a href="#" :class="[active ? 'bg-gray-50' : '', 'block px-3 py-1 text-sm leading-6 text-gray-900']">Date updated</a>
                </MenuItem>
                <MenuItem v-slot="{ active }">
                <a href="#" :class="[active ? 'bg-gray-50' : '', 'block px-3 py-1 text-sm leading-6 text-gray-900']">Environment</a>
                </MenuItem>
            </MenuItems>
            </transition>
        </Menu>
        </header>

                <div>
            jonas
            <input type="file"
                id="camera"
                name="camera"
                accept="image/*"
                capture="user"
            />

            <input type="file"
                id="picture"
                name="picture"
                accept="image/*"
                capture="environment"
            />
        </div>


        <ul role="list" class="divide-y divide-white/5">
        <li v-for="deployment in deployments" :key="deployment.id" class="relative flex items-center space-x-4 px-4 py-4 sm:px-6 lg:px-8">
            <div class="min-w-0 flex-auto">
            <div class="flex items-center gap-x-3">
                <div :class="[statuses[deployment.status], 'flex-none rounded-full p-1']">
                <div class="h-2 w-2 rounded-full bg-current" />
                </div>
                <h2 class="min-w-0 text-sm font-semibold leading-6 text-white">
                <a :href="deployment.href" class="flex gap-x-2">
                    <span class="truncate">{{ deployment.teamName }}</span>
                    <span class="text-gray-400">/</span>
                    <span class="whitespace-nowrap">{{ deployment.projectName }}</span>
                    <span class="absolute inset-0" />
                </a>
                </h2>
            </div>
            <div class="mt-3 flex items-center gap-x-2.5 text-xs leading-5 text-gray-400">
                <p class="truncate">{{ deployment.description }}</p>
                <svg viewBox="0 0 2 2" class="h-0.5 w-0.5 flex-none fill-gray-300">
                <circle cx="1" cy="1" r="1" />
                </svg>
                <p class="whitespace-nowrap">{{ deployment.statusText }}</p>
            </div>
            </div>
            <div :class="[environments[deployment.environment], 'flex-none rounded-full px-2 py-1 text-xs font-medium ring-1 ring-inset']">{{ deployment.environment }} {{ deployment.timing }}</div>
            <ChevronRightIcon class="h-5 w-5 flex-none text-gray-400" aria-hidden="true" />
        </li>
        </ul>
    </main>

    <aside class="bg-black/10 lg:fixed lg:bottom-0 lg:right-0 lg:top-16 lg:w-96 lg:overflow-y-auto lg:border-l lg:border-white/5">
        <header class="flex items-center justify-between border-b border-white/5 px-4 py-4 sm:px-6 sm:py-6 lg:px-8">
        <h2 class="text-base font-semibold leading-7 text-white">Activity feed</h2>
        <a href="#" class="text-sm font-semibold leading-6 text-indigo-400">View all</a>
        </header>
        <ul role="list" class="divide-y divide-white/5">
        <li v-for="item in activityItems" :key="item.commit" class="px-4 py-4 sm:px-6 lg:px-8">
            <div class="flex items-center gap-x-3">
            <img :src="item.user.imageUrl" alt="" class="h-6 w-6 flex-none rounded-full bg-gray-800" />
            <h3 class="flex-auto truncate text-sm font-semibold leading-6 text-white">{{ item.user.name }}</h3>
            <time :datetime="item.dateTime" class="flex-none text-xs text-gray-600">{{ item.date }}</time>
            </div>
            <p class="mt-3 truncate text-sm text-gray-500">
            Pushed to <span class="text-gray-400">{{ item.projectName }}</span> (<span class="font-mono text-gray-400">{{ item.commit }}</span> on <span class="text-gray-400">{{ item.branch }}</span
            >)
            </p>
        </li>
        </ul>
    </aside>
</template>

<script setup>
import { ref, onMounted} from 'vue'
import {getData, db} from '../services/firebase'
import { doc, onSnapshot } from 'firebase/firestore';
import {
    Dialog,
    DialogPanel,
    Menu,
    MenuButton,
    MenuItem,
    MenuItems,
    TransitionChild,
    TransitionRoot,
  } from '@headlessui/vue'
  import {
    ChartBarSquareIcon,
    Cog6ToothIcon,
    FolderIcon,
    GlobeAltIcon,
    ServerIcon,
    SignalIcon,
    XMarkIcon,
  } from '@heroicons/vue/24/outline'
  import { Bars3Icon, ChevronRightIcon, ChevronUpDownIcon, MagnifyingGlassIcon } from '@heroicons/vue/20/solid'
  
  const navigation = [
    { name: 'Projects', href: '#', icon: FolderIcon, current: false },
    { name: 'Deployments', href: '#', icon: ServerIcon, current: true },
  ]
  const teams = [
    { id: 1, name: 'Planetaria', href: '#', initial: 'P', current: false },
    { id: 2, name: 'Protocol', href: '#', initial: 'P', current: false },
    { id: 3, name: 'Tailwind Labs', href: '#', initial: 'T', current: false },
  ]
  const statuses = {
    offline: 'text-gray-500 bg-gray-100/10',
    online: 'text-green-400 bg-green-400/10',
    error: 'text-rose-400 bg-rose-400/10',
  }
  const environments = {
    Preview: 'text-gray-400 bg-gray-400/10 ring-gray-400/20',
    Production: 'text-indigo-400 bg-indigo-400/10 ring-indigo-400/30',
  }
  const deployments = ref([])
  const activityItems = [
    {
      user: {
        name: 'Michael Foster',
        imageUrl:
          'https://images.unsplash.com/photo-1519244703995-f4e0f30006d5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80',
      },
      projectName: 'Number Participant',
      commit: '2d89f0c8',
      branch: 'main',
      date: '1h',
      dateTime: '2023-01-23T11:00',
    },
    // More items...
  ]
  
  const sidebarOpen = ref(false)

  onMounted(async()=>{
    const data = await getData();

    deployments.value = data.map(e=>({
      id: e.id,
      projectName: e.number,
      teamName: e.name,
      status: e.status,
      statusText: 'Initiated 8:00',
      description: e.personalData,
      environment: 'Preview',
      timing: e.timing
    }))

    const unsub = onSnapshot(doc(db, "people", "SF"), (doc) => {
      console.log("Current data: ", doc.data());
    });
  })  

</script>
