<template>
  <div class="h-screen bg-gray-50 dark:bg-gray-950 flex font-sans overflow-hidden">
    <!-- Mobile Sidebar Backdrop -->
    <div
      v-if="isSidebarOpen"
      class="fixed inset-0 z-40 bg-gray-900/50 backdrop-blur-sm lg:hidden"
      @click="isSidebarOpen = false"
    />

    <!-- Sidebar (Floating/Behind Style) -->
    <aside
      :class="[
        'fixed inset-y-0 left-0 z-50 bg-slate-50 dark:bg-gray-950 transform transition-all duration-300 lg:static lg:inset-auto overflow-hidden',
        isSidebarOpen ? 'translate-x-0' : '-translate-x-full lg:translate-x-0',
        isCollapsed ? 'lg:w-20' : 'lg:w-64',
        'w-64'
      ]"
    >
      <div class="h-full flex flex-col p-4">
        <!-- Brand & Toggle -->
        <div class="flex items-center transition-all duration-300 relative py-2">
          
          <!-- Logo -->
          <div 
            class="flex items-center justify-center overflow-hidden w-full relative" 
            :class="[
              isCollapsed ? 'cursor-pointer h-20' : 'h-28',
            ]"
            @click="isCollapsed && (isCollapsed = false)"
          >
           
            <BrandLogo :collapsed="isCollapsed" />

            <!-- Toggle Button (Top Right Overlay) -->
            <button
              @click.stop="isCollapsed = !isCollapsed"
              class="absolute -top-1 right-0 w-6 h-6 flex items-center justify-center rounded-lg transition-colors group z-20 text-gray-400 hover:text-gray-600 dark:text-gray-500 dark:hover:text-gray-300"
            >
               <UIcon 
                 :name="isCollapsed ? 'i-heroicons-chevron-right' : 'i-heroicons-chevron-left'" 
                 class="w-4 h-4 group-hover:scale-110 transition-transform" 
               />
            </button>
            
          </div>
        </div>

        <!-- Navigation -->
        <!-- Navigation (Accordion) -->
        <nav class="flex-1 overflow-y-auto space-y-1 mt-4">
           <!-- Home Item -->
           <NuxtLink 
            to="/" 
            class="flex items-center gap-2 px-3 py-2 text-[15px] font-medium rounded-md transition-colors w-full group focus:outline-none focus-visible:outline-none dark:focus-visible:outline-none"
            :class="[
               $route.path === '/' 
                 ? 'text-gray-900 dark:text-white bg-gray-100 dark:bg-gray-800' 
                 : 'text-gray-500 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white hover:bg-gray-50 dark:hover:bg-gray-800'
            ]"
            :title="isCollapsed ? 'Início' : ''"
           >
              <UIcon name="i-heroicons-home" class="w-5 h-5 flex-shrink-0" />
              <span v-if="!isCollapsed" class="truncate">Início</span>
           </NuxtLink>

           <UAccordion 
            :items="accordionItems" 
            :ui="{ 
              wrapper: 'w-full',
              item: { padding: 'pt-1.5 pb-1.5' },
              button: { 
                base: 'flex items-center gap-1.5 group w-full focus:outline-none focus-visible:outline-none dark:focus-visible:outline-none border-t border-transparent',
                active: 'text-gray-900 dark:text-white bg-gray-50 dark:bg-gray-800',
                inactive: 'text-gray-500 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white',
                padding: 'py-2.5 px-3'
              } 
            }"
          >
            <template #default="{ item, open }">
               <UButton color="gray" variant="ghost" class="w-full justify-between hover:bg-gray-100 dark:hover:bg-gray-800 text-[15px]" :class="[open ? 'bg-gray-50 dark:bg-gray-800' : '']">
                  <div class="flex items-center gap-2">
                    <UIcon :name="item.icon" class="w-5 h-5" />
                    <span v-if="!isCollapsed" class="truncate">{{ item.label }}</span>
                  </div>
                   <UIcon 
                    v-if="!isCollapsed" 
                    name="i-heroicons-chevron-right-20-solid" 
                    class="w-4 h-4 transform transition-transform duration-200" 
                    :class="[open && 'rotate-90']" 
                  />
               </UButton>
            </template>
            
            <template #pessoas>
               <div v-if="!isCollapsed" class="px-2 py-1 space-y-1">
                  <UButton v-for="link in pessoasLinks" :key="link.label" :to="link.to" variant="ghost" color="gray" size="sm" block class="justify-start pl-9 text-gray-500 hover:text-gray-900">
                    {{ link.label }}
                  </UButton>
               </div>
            </template>
            <template #modulos>
               <div v-if="!isCollapsed" class="px-2 py-1 space-y-1">
                  <UButton v-for="link in modulosLinks" :key="link.label" :to="link.to" variant="ghost" color="gray" size="sm" block class="justify-start pl-9 text-gray-500 hover:text-gray-900">
                    {{ link.label }}
                  </UButton>
               </div>
            </template>
            <template #servicos>
                <div v-if="!isCollapsed" class="px-2 py-1 space-y-1">
                  <UButton v-for="link in servicosLinks" :key="link.label" :to="link.to" variant="ghost" color="gray" size="sm" block class="justify-start pl-9 text-gray-500 hover:text-gray-900">
                    {{ link.label }}
                  </UButton>
               </div>
            </template>
            <template #config>
                <div v-if="!isCollapsed" class="px-2 py-1 space-y-1">
                  <UButton v-for="link in configLinks" :key="link.label" :to="link.to" variant="ghost" color="gray" size="sm" block class="justify-start pl-9 text-gray-500 hover:text-gray-900">
                    {{ link.label }}
                  </UButton>
               </div>
            </template>
          </UAccordion>
        </nav>

        <!-- Sidebar Footer -->
        <!-- Sidebar Footer -->
        <div class="mt-auto py-4 flex flex-col gap-4 px-4" :class="isCollapsed ? 'items-center' : 'items-end'">
           <div class="flex items-center gap-2" :class="isCollapsed ? 'justify-center' : 'justify-end w-full'">
             <UTooltip text="Central de Ajuda" :popper="{ placement: 'right' }">
              <UButton
                icon="i-heroicons-question-mark-circle"
                color="gray"
                variant="ghost"
                size="lg"
                class="text-gray-400 hover:text-gray-600 dark:text-gray-500 dark:hover:text-gray-300"
              />
            </UTooltip>
           </div>
        </div>
      </div>
    </aside>

    <!-- Main Content Area (Normal / Full Screen) -->
    <div class="flex-1 flex flex-col h-full min-w-0 transition-all duration-300 bg-white dark:bg-gray-900 shadow-[-8px_0_24px_-4px_rgba(0,0,0,0.2)] dark:shadow-[-8px_0_24px_-4px_rgba(0,0,0,0.4)] relative z-10">
      
        <!-- Mobile Toggle (Absolute) -->
         <div class="lg:hidden absolute top-4 left-4 z-20">
            <button @click="isSidebarOpen = true" class="p-2 text-gray-500">
              <UIcon name="i-heroicons-bars-3" class="h-6 w-6" />
            </button>
         </div>

        <!-- Header -->
        <header class="flex-none flex items-center justify-between h-16 px-6 border-b border-gray-200 dark:border-gray-800">
          <!-- Left: Breadcrumbs -->
          <div class="flex items-center gap-4 lg:ml-0 ml-12">
             <div class="flex items-center gap-2 text-sm text-gray-500 dark:text-gray-400">
                <NuxtLink to="/" class="flex items-center hover:text-blue-500 transition-colors">
                  <UIcon name="i-heroicons-home" class="w-4 h-4" />
                </NuxtLink>
                <UIcon name="i-heroicons-chevron-right" class="w-3 h-3 text-gray-300" />
                <span class="font-medium text-gray-900 dark:text-white">Início</span>
             </div>
          </div>

          <!-- Right Actions -->
          <div class="flex items-center gap-3">
            <!-- Search -->
            <UTooltip text="Pesquisar" :shortcuts="['⌘', 'K']">
               <UButton 
                 icon="i-heroicons-magnifying-glass" 
                 color="gray" 
                 variant="ghost" 
                 class="rounded-md hover:bg-gray-100 dark:hover:bg-gray-800" 
                 @click="isSearchOpen = true" 
               />
            </UTooltip>

            <!-- Theme Toggle -->
            <ClientOnly>
              <UButton
                :icon="isDark ? 'i-heroicons-moon' : 'i-heroicons-sun'"
                color="gray"
                variant="ghost"
                class="rounded-md"
                @click="toggleTheme"
              />
            </ClientOnly>
            
            <!-- Notifications Trigger -->
            <UButton
              icon="i-heroicons-bell"
              color="gray"
              variant="ghost"
              class="rounded-full relative hover:bg-gray-100 dark:hover:bg-gray-800 w-10 h-10 flex items-center justify-center transition-colors"
              @click="isNotificationsOpen = true"
            >
              <UIcon name="i-heroicons-bell" class="w-6 h-6 text-gray-600 dark:text-gray-300" />
              <!-- Badge with cut-out border -->
              <span v-if="unreadCount > 0" class="absolute top-2 right-2 flex h-2.5 w-2.5">
                <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-red-400 opacity-75"></span>
                <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-red-500 ring-2 ring-white dark:ring-gray-900"></span>
              </span>
            </UButton>

            <!-- User Dropdown -->
            <UDropdown :items="userDropdownItems" :ui="{ item: { disabled: 'cursor-text select-text' } }" :popper="{ placement: 'bottom-end' }">
              <UAvatar
                src="https://i.pravatar.cc/150?u=delorean"
                alt="User Avatar"
                size="sm"
                class="ring-2 ring-white dark:ring-gray-800 cursor-pointer bg-gray-100 dark:bg-gray-800"
              />

              <template #account="{ item }">
                <div class="px-3 py-2.5">
                  <p class="truncate text-sm font-medium text-gray-700 dark:text-gray-200">
                    {{ item.label }}
                  </p>
                </div>
              </template>

              <template #item="{ item }">
                <span class="truncate">{{ item.label }}</span>
                <UIcon :name="item.icon" class="flex-shrink-0 h-4 w-4 ms-auto text-gray-400 dark:text-gray-500" />
              </template>
            </UDropdown>
          </div>
        </header>

        <!-- Main Scrollable Content -->
        <main class="flex-1 overflow-y-auto p-6">
          <slot />
        </main>
    </div>
    <!-- Notifications Slideover -->
    <USlideover v-model="isNotificationsOpen" :ui="{ width: 'w-screen max-w-md' }">
      <div class="flex flex-col h-full bg-white dark:bg-gray-900 shadow-2xl">
        
        <!-- Header -->
        <div class="flex items-center justify-between p-5 border-b border-gray-100 dark:border-gray-800">
          <div class="flex items-center gap-3">
             <h3 class="text-xl font-bold text-gray-900 dark:text-white">Notificações</h3>
             <UBadge v-if="unreadCount > 0" label="2 Novas" variant="subtle" color="blue" size="xs" />
          </div>
          <div class="flex items-center gap-1">
             <UTooltip text="Marcar todas como lidas">
                <UButton icon="i-heroicons-check-circle" color="gray" variant="ghost" class="text-gray-400 hover:text-blue-500" />
             </UTooltip>
             <UButton icon="i-heroicons-x-mark" color="gray" variant="ghost" class="text-gray-400 hover:text-gray-600" @click="isNotificationsOpen = false" />
          </div>
        </div>

        <!-- Search & Filter -->
        <div class="p-4 pb-0 bg-gray-50/50 dark:bg-gray-800/20 space-y-4">
           <UInput 
             icon="i-heroicons-magnifying-glass" 
             placeholder="Pesquisar notificações..." 
             color="white" 
             variant="outline" 
             :ui="{ icon: { trailing: { pointer: '' } } }"
             class="w-full bg-gray-50 dark:bg-gray-800 border-gray-200 dark:border-gray-700"
           />
        </div>

        <!-- Tabs & List -->
        <div class="flex-1 overflow-hidden flex flex-col">
            <UTabs 
               :items="notificationTabs" 
               class="w-full flex-1 flex flex-col" 
               :ui="{ 
                  list: { background: 'bg-transparent', tab: { active: 'text-blue-600 dark:text-blue-400', inactive: 'text-gray-500' }, padding: 'px-4' },
                  wrapper: 'flex-1 flex flex-col overflow-hidden' 
               }"
            >
                 <template #geral="{ item }">
                    <div class="overflow-y-auto p-4 space-y-2 h-full">
                      <!-- Date Header -->
                      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wider mb-2 mt-1">Hoje</p>

                      <div 
                        v-for="msg in messages" 
                        :key="msg.id" 
                        class="group relative bg-white dark:bg-gray-900 p-4 rounded-xl border border-gray-100 dark:border-gray-800 hover:bg-gray-50 dark:hover:bg-gray-800 transition-all cursor-pointer"
                        :class="msg.unread ? 'bg-blue-50/30' : ''"
                      >
                         <div class="flex gap-3">
                             <UAvatar size="sm" :alt="msg.sender" class="bg-blue-100 text-blue-600" />
                             <div class="flex-1 min-w-0">
                                 <div class="flex justify-between items-start">
                                     <p class="text-sm font-semibold text-gray-900 dark:text-white whitespace-normal break-words">
                                       {{ msg.sender }}
                                       <span class="font-normal text-gray-500">adicionou um comentário em</span>
                                       <span class="font-medium text-blue-600"> {{ msg.title }}</span>
                                     </p>
                                     <div class="text-[10px] text-gray-400 whitespace-nowrap ml-2 shrink-0">{{ msg.date }}</div>
                                 </div>
                                 
                                 <!-- Voice Message Mockup (Visual Polish) -->
                                 <div v-if="msg.id === 101" class="mt-2 flex items-center gap-2 bg-gray-50 dark:bg-gray-800 p-2 rounded-lg w-fit">
                                    <UIcon name="i-heroicons-play-circle" class="w-6 h-6 text-blue-500" />
                                    <div class="h-1 w-24 bg-gray-200 dark:bg-gray-700 rounded-full overflow-hidden">
                                       <div class="h-full w-1/3 bg-blue-500"></div>
                                    </div>
                                    <span class="text-xs text-gray-500">0:58</span>
                                 </div>
                                 <p v-else class="text-xs text-gray-500 dark:text-gray-400 mt-1 line-clamp-2 whitespace-normal break-words">
                                    Esta é uma prévia da mensagem para contextualizar o usuário rapidamente sobre o conteúdo.
                                 </p>
                             </div>
                         </div>
                         
                         <!-- Hover Actions -->
                         <div class="absolute top-2 right-2 opacity-0 group-hover:opacity-100 transition-opacity flex gap-1 bg-white dark:bg-gray-900 pl-2 rounded-bl-lg">
                            <UButton icon="i-heroicons-check" size="2xs" color="gray" variant="ghost" class="hover:text-blue-500" />
                            <UButton icon="i-heroicons-archive-box" size="2xs" color="gray" variant="ghost" class="hover:text-gray-700" />
                         </div>
                         
                         <!-- Unread Dot -->
                         <div v-if="msg.unread" class="absolute left-1 top-1/2 -translate-y-1/2 w-1.5 h-1.5 rounded-full bg-blue-500"></div>
                      </div>
                    </div>
                 </template>

                 <template #editais="{ item }">
                    <div class="overflow-y-auto p-4 space-y-2 h-full">
                       <p class="text-xs font-semibold text-gray-400 uppercase tracking-wider mb-2 mt-1">Esta Semana</p>
                      <div class="bg-white dark:bg-gray-900/50 p-4 rounded-xl border border-gray-100 dark:border-gray-800 flex gap-4">
                         <div class="flex flex-col items-center justify-center w-12 h-12 bg-blue-50 text-blue-600 rounded-lg shrink-0">
                            <span class="text-xs font-bold uppercase">FEV</span>
                            <span class="text-lg font-bold">20</span>
                         </div>
                         <div class="flex-1">
                            <h4 class="text-sm font-bold text-gray-900 dark:text-white">Inscrições Edital 04/2026</h4>
                            <p class="text-xs text-gray-500 mt-1">Data limite para submissão de propostas de inovação.</p>
                            <UButton label="Ver Edital" size="2xs" variant="soft" class="mt-2" color="blue" />
                         </div>
                      </div>
                    </div>
                 </template>

                 <template #avisos="{ item }">
                    <div class="overflow-y-auto p-4 space-y-2 h-full">
                       <div class="bg-red-50 dark:bg-red-900/10 p-4 rounded-xl border border-red-100 dark:border-red-900/30 flex gap-3">
                          <UIcon name="i-heroicons-exclamation-triangle" class="w-5 h-5 text-red-500 shrink-0" />
                          <div>
                             <h4 class="text-sm font-bold text-gray-900 dark:text-white">Manutenção Programada</h4>
                             <p class="text-xs text-gray-600 dark:text-gray-400 mt-1">O sistema ficará indisponível nesta sexta-feira das 00:00 às 06:00 para atualizações de segurança.</p>
                          </div>
                       </div>
                    </div>
                 </template>
            </UTabs>
        </div>
      </div>
    </USlideover>

    <!-- Search Spotlight Modal -->
    <UModal v-model="isSearchOpen" :ui="{ width: 'sm:max-w-xl', overlay: { background: 'bg-gray-900/40 backdrop-blur-sm' } }">
       <div class="flex flex-col bg-white dark:bg-gray-900 overflow-hidden">
          <!-- Search Input -->
          <div class="p-4 border-b border-gray-100 dark:border-gray-800 flex items-center gap-3">
             <UIcon name="i-heroicons-magnifying-glass" class="w-6 h-6 text-gray-400" />
             <input 
               type="text" 
               placeholder="Pesquisar por pessoas, projetos ou editais..." 
               class="flex-1 bg-transparent border-none text-lg text-gray-900 dark:text-white placeholder-gray-400 focus:outline-none"
               autofocus
             />
             <UKbd>ESC</UKbd>
          </div>
          
          <!-- Suggestions -->
          <div class="p-2">
             <div class="px-2 py-1.5 text-xs font-semibold text-gray-400 uppercase tracking-wider">Acesso Rápido</div>
             <div class="space-y-1">
                <UButton variant="ghost" color="gray" icon="i-heroicons-plus" block class="justify-start text-gray-600 dark:text-gray-300">Novo Processo</UButton>
                <UButton variant="ghost" color="gray" icon="i-heroicons-ticket" block class="justify-start text-gray-600 dark:text-gray-300">Minhas Bolsas</UButton>
                <UButton variant="ghost" color="gray" icon="i-heroicons-document-text" block class="justify-start text-gray-600 dark:text-gray-300">Editais Abertos</UButton>
             </div>
          </div>
       </div>
    </UModal>
  </div>
</template>

<script setup lang="ts">
const colorMode = useColorMode()
const isSidebarOpen = ref(false)
const isCollapsed = ref(false)
const isNotificationsOpen = ref(false)
const isSearchOpen = ref(false)

const isDark = computed(() => colorMode.value === 'dark')

// Navigation Groups
const accordionItems = [
  { label: 'Pessoas', icon: 'i-heroicons-users', slot: 'pessoas', defaultOpen: true },
  { label: 'Módulos', icon: 'i-heroicons-squares-2x2', slot: 'modulos' },
  { label: 'Serviços', icon: 'i-heroicons-clipboard-document-check', slot: 'servicos' },
  { label: 'Configurações', icon: 'i-heroicons-cog-6-tooth', slot: 'config' }
]

const pessoasLinks = [
  { label: 'Pesquisadores', to: '#' },
  { label: 'Bolsistas', to: '#' },
  { label: 'Consultores', to: '#' },
  { label: 'Pesq. Especial Evento', to: '#' },
  { label: 'Comissões Setoriais', to: '#' }
]

const modulosLinks = [
  { label: 'Termos Jurídicos', to: '/termos' },
  { label: 'Programas', to: '/programas' },
  { label: 'Editais', to: '/editais' },
  { label: 'Propostas', to: '/propostas' },
  { label: 'Processos', to: '/processos' },
  { label: 'Instituições', to: '#' },
  { label: 'Patrimônio', to: '#' }
]

const servicosLinks = [
  { label: 'Correio', to: '#' },
  { label: 'Relatórios', to: '#' },
  { label: 'Ofícios', to: '#' },
  { label: "CI's", to: '#' },
  { label: 'Documentos', to: '#' }
]

const configLinks = [
  { label: 'Configurações Iniciais', to: '#' },
  { label: 'Certificados', to: '#' },
  { label: 'Usuários', to: '#' },
  { label: 'Permissões', to: '#' }
]

function toggleTheme() {
  colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
}

// User Dropdown Items
const userDropdownItems = [
  [{
    label: 'maria@gmail.com',
    slot: 'account',
    disabled: true
  }],
  [{
    label: 'Meu Perfil',
    icon: 'i-heroicons-user'
  }, {
    label: 'Configurações',
    icon: 'i-heroicons-cog-8-tooth'
  }],
  [{
    label: 'Sair',
    icon: 'i-heroicons-arrow-left-on-rectangle'
  }]
]

// Notifications Data (Moved from index.vue)
interface Message {
  id: number
  title: string
  date: string
  sender?: string
  unread?: boolean
}

// Breadcrumbs
const breadcrumbLinks = [
  { icon: 'i-heroicons-home', label: 'Início', to: '/' }
]

const messages = ref<Message[]>([
  {
    id: 101,
    title: 'Dúvida sobre Processo 4421',
    sender: 'Maria Silva',
    date: '10 min',
    unread: true
  },
  {
    id: 102,
    title: 'Aprovação de Relatório',
    sender: 'Sistema',
    date: '2h',
    unread: false
  },
  {
    id: 103,
    title: 'Solicitação de Reunião',
    sender: 'Dr. Roberto Almeida',
    date: 'Ontem',
    unread: false
  }
])

const unreadCount = computed(() => messages.value.filter(m => m.unread).length)

// Notification Tabs
const notificationTabs = computed(() => [
  { label: `Geral (${unreadCount.value})`, slot: 'geral' },
  { label: 'Editais (2)', slot: 'editais' },
  { label: 'Avisos (1)', slot: 'avisos' }
])
</script>
