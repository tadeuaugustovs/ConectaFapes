<template>
  <div class="space-y-4">
    <!-- Section A: Operational Indicators (Moved to Top) -->
    <section>
      <h2 class="text-base font-medium text-gray-500 dark:text-gray-400 mb-2">Indicadores Operacionais</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
        <DashboardStatWidget
          label="Bolsas Ativas"
          value="1,248"
        />
        <DashboardStatWidget
          label="Projetos em Andamento"
          value="86"
        />
        <DashboardStatWidget
          label="Pesquisadores Vinculados"
          value="40,044"
        />
      </div>
    </section>

    <!-- Section B: Quick Action Hub (Moved Below Stats) -->
    <section>
      <h2 class="text-base font-semibold text-gray-900 dark:text-white mb-3">Acesso Rápido</h2>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
        <DashboardActionCard
          title="Solicitar Bolsa"
          description="Novo pedido de bolsa."
          icon="i-heroicons-academic-cap"
          @click="actionClick('Solicitar Bolsa')"
        />
        <DashboardActionCard
          title="Enviar Relatório"
          description="Relatórios pendentes."
          icon="i-heroicons-document-text"
          @click="actionClick('Enviar Relatório')"
        />
        <DashboardActionCard
          title="Consultar Editais"
          description="Editais abertos."
          icon="i-heroicons-megaphone"
          @click="actionClick('Consultar Editais')"
        />
        <DashboardActionCard
          title="Novo Processo"
          description="Novo processo adm."
          icon="i-heroicons-folder-plus"
          @click="actionClick('Novo Processo')"
        />
      </div>
    </section>

    <!-- Section C: Project Updates (Boxed) -->
    <section class="bg-white dark:bg-gray-900 rounded-md border border-gray-200 dark:border-gray-800 overflow-hidden">
      <div class="p-3 border-b border-gray-200 dark:border-gray-800 flex items-center justify-between">
        <h2 class="text-sm font-semibold text-gray-900 dark:text-white">Atualizações do Projeto</h2>
        <UButton variant="ghost" color="gray" size="xs" to="#" icon="i-heroicons-arrow-right" class="text-gray-400 hover:text-gray-600" />
      </div>

      <div class="divide-y divide-gray-100 dark:divide-gray-800">
        <div
          v-for="update in projectUpdates.slice(0,3)"
          :key="update.id"
          class="p-3 hover:bg-gray-50 dark:hover:bg-gray-800/50 transition-colors cursor-pointer group flex gap-3 items-start"
          @click="openMessage(update)"
        >
          <!-- Timeline-like Icon/Dot -->
           <div class="mt-1.5 w-2 h-2 rounded-full bg-blue-500 ring-4 ring-blue-50 dark:ring-blue-900/30 flex-shrink-0"></div>

          <div class="flex-1 min-w-0">
             <div class="flex justify-between items-start">
                <h4 class="text-sm font-medium text-gray-900 dark:text-white group-hover:text-blue-600 dark:group-hover:text-blue-400 transition-colors">
                  {{ update.title }}
                </h4>
                <span class="text-xs text-gray-400 whitespace-nowrap ml-2 bg-gray-50 dark:bg-gray-800 px-2 py-0.5 rounded-full">{{ update.date }}</span>
             </div>
             <p class="mt-0.5 text-xs text-gray-500 dark:text-gray-400 line-clamp-1 leading-relaxed">
               {{ update.summary }}
             </p>
          </div>
        </div>
      </div>
    </section>

    <!-- Section D: Your Grants (Boxed) -->
    <section class="bg-white dark:bg-gray-900 rounded-md border border-gray-200 dark:border-gray-800 overflow-hidden">
      <div class="p-3 border-b border-gray-200 dark:border-gray-800 flex items-center justify-between">
         <h2 class="text-sm font-semibold text-gray-900 dark:text-white">Suas Bolsas</h2>
         <UButton variant="ghost" color="gray" size="xs" to="#" icon="i-heroicons-arrow-right" class="text-gray-400 hover:text-gray-600" />
      </div>
      
      <div class="divide-y divide-gray-100 dark:divide-gray-800">
        <div 
          v-for="bolsa in bolsas" 
          :key="bolsa.id"
          class="p-3 hover:bg-gray-50 dark:hover:bg-gray-800/50 transition-colors flex items-center justify-between group"
        >
           <div class="flex items-center gap-4">
              <div>
                 <p class="font-semibold text-gray-900 dark:text-white text-sm group-hover:text-blue-600 transition-colors">{{ bolsa.title }}</p>
                 <p class="text-xs text-gray-500 dark:text-gray-400 mt-0.5 flex items-center gap-1">
                    <UIcon name="i-heroicons-calendar" class="w-3 h-3" />
                    {{ bolsa.period }}
                 </p>
              </div>
           </div>
           
           <div class="flex items-center gap-2">
             <UBadge :color="bolsa.status === 'Vigente' ? 'green' : 'gray'" variant="subtle" size="xs" :ui="{ rounded: 'rounded-full' }">
               {{ bolsa.status }}
             </UBadge>
             <UButton icon="i-heroicons-chevron-right" color="gray" variant="ghost" size="xs" class="text-gray-300 group-hover:text-gray-500" />
           </div>
        </div>
      </div>
    </section>

    <!-- Message Detail Modal -->
    <UModal v-model="isModalOpen">
      <UCard :ui="{ ring: '', divide: 'divide-y divide-gray-100 dark:divide-gray-800' }">
        <template #header>
          <div class="flex items-center justify-between">
            <h3 class="text-base font-semibold leading-6 text-gray-900 dark:text-white">
              {{ selectedMessage?.title }}
            </h3>
            <UButton color="gray" variant="ghost" icon="i-heroicons-x-mark-20-solid" class="-my-1" @click="isModalOpen = false" />
          </div>
        </template>

        <div class="py-2">
          <div class="mb-4 text-sm text-gray-500 dark:text-gray-400 flex justify-between">
              <span>{{ selectedMessage?.sender || 'Administração' }}</span>
              <span>{{ selectedMessage?.date }}</span>
          </div>
          <p class="text-sm text-gray-700 dark:text-gray-300 whitespace-pre-line">
            {{ selectedMessage?.body }}
          </p>
        </div>

        <template #footer>
          <div class="flex justify-end">
             <UButton color="gray" variant="solid" @click="isModalOpen = false">Fechar</UButton>
          </div>
        </template>
      </UCard>
    </UModal>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: 'dashboard'
})

// Types
interface Message {
  id: number
  title: string
  date: string
  summary?: string
  sender?: string
  body: string
  unread?: boolean
}

interface Bolsa {
  id: number
  title: string
  period: string
  status: 'Vigente' | 'Finalizada'
}

// State
const isModalOpen = ref(false)
const selectedMessage = ref<Message | null>(null)

// Actions
const actionClick = (actionName: string) => {
  console.log('Action clicked:', actionName)
  // In a real app, this would navigate or open a specific modal
}

const openMessage = (msg: Message) => {
  selectedMessage.value = msg
  isModalOpen.value = true
}

// Dummy Data
const projectUpdates: Message[] = [
  {
    id: 1,
    title: 'Atualização no Sistema de Bolsas',
    summary: 'O módulo de solicitação de bolsas passará por manutenção programada.',
    date: 'Hoje, 09:00',
    body: 'Prezados Coordenadores,\n\nInformamos que o módulo de solicitação de bolsas passará por manutenção programada nesta sexta-feira, das 00:00 às 06:00. Durante este período, o sistema ficará indisponível.\n\nAgradecemos a compreensão.\nEquipe de TI FAPES'
  },
  {
    id: 2,
    title: 'Novo Edital de Inovação',
    summary: 'Publicado o edital 04/2026 focado em startups e inovação tecnológica.',
    date: 'Ontem',
    body: 'O Edital 04/2026 já está disponível para consulta. Este edital visa apoiar projetos de inovação tecnológica em parceria com startups.\n\nConsulte os detalhes na seção de Editais.'
  },
  {
    id: 3,
    title: 'Prazo para Relatórios',
    summary: 'Lembrete: o prazo final para envio dos relatórios anuais é 30/01.',
    date: '18 Jan',
    body: 'Lembramos a todos os coordenadores que o prazo final para o envio dos relatórios anuais de atividades se encerra no dia 30/01. Não haverá prorrogação.\n\nAtenciosamente,\nDiretoria Científica'
  }
]

const bolsas: Bolsa[] = [
  {
    id: 1,
    title: 'Bolsa de Iniciação Científica - Edital 03/2024',
    period: '26.08.2024 - 26.08.2025',
    status: 'Vigente'
  },
  {
    id: 2,
    title: 'Auxílio Participação em Evento - COBRAC 2023',
    period: '10.05.2023 - 15.05.2023',
    status: 'Finalizada'
  }
]
</script>
