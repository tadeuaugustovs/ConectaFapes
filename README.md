# Conecta FAPES - Interface

Este repositório contém a nova implementação do front-end para o sistema **Conecta FAPES**. O projeto foi desenvolvido com foco em modernização da experiência do usuário, alta performance e adesão a padrões de design corporativo (*Enterprise Grade Design*).

## Sobre o Projeto

O objetivo desta refatoração é entregar uma interface robusta, densa e intuitiva para coordenadores e pesquisadores. A nova UI abandona o estilo visual legado em favor de uma abordagem minimalista e orientada a dados, inspirada em sistemas de gestão modernos (como Microsoft Fluent e Vercel).

### Conceito Visual e UX

* **Densidade de Informação:** O layout foi otimizado para exibir indicadores operacionais, ações rápidas e atualizações críticas "acima da dobra" (*above the fold*), eliminando a necessidade de rolagem excessiva em monitores padrão.
* **Design Sóbrio e Funcional:** Utilização de bordas sutis e alto contraste para definição de hierarquia, evitando sombras excessivas ou elementos decorativos desnecessários.
* **Modo Escuro Nativo:** O sistema foi projetado desde o início com suporte de primeira classe ao *Dark Mode*, garantindo conforto visual e consistência de cores em ambos os temas.
* **Navegação Contextual:** Menu lateral colapsável com hierarquia clara e busca global inteligente (*Spotlight*) para acesso rápido a funções profundas do sistema.

* <img width="1853" height="962" alt="Captura de tela de 2026-01-21 01-29-19" src="https://github.com/user-attachments/assets/b2a5cc75-0dcb-4de9-9d0e-e179c711ae9b" />
<img width="1853" height="962" alt="Captura de tela de 2026-01-21 01-29-29" src="https://github.com/user-attachments/assets/a30f5129-91f7-41ed-bffc-895aa0d46693" />



## Stack Tecnológico

O projeto foi construído utilizando as tecnologias mais recentes do ecossistema Vue.js, garantindo escalabilidade e facilidade de manutenção.

* **Framework:** [Nuxt 3](https://nuxt.com/) (Vue 3 + Composition API)
* **UI Library:** [Nuxt UI](https://ui.nuxt.com/)
* **Estilização:** [Tailwind CSS](https://tailwindcss.com/)
* **Gerenciamento de Estado:** [Pinia](https://pinia.vuejs.org/)
* **Ícones:** Heroicons (via Nuxt UI)

## Funcionalidades Implementadas

1.  **Dashboard Operacional:** Visão geral com métricas de bolsas ativas, projetos e pesquisadores.
2.  **Acesso Rápido:** Atalhos para as funções mais utilizadas (Solicitar Bolsa, Enviar Relatório).
3.  **Sistema de Notificações:** Painel lateral (*Slideover*) para gestão de avisos e comunicados.
4.  **Gestão de Perfil:** Menu de usuário simplificado e seguro.
5.  **Responsividade Total:** Layout adaptável para Mobile (com menu *drawer*) e Desktop.

## Como Executar

Certifique-se de ter o [Node.js](https://nodejs.org/) instalado.

### 1. Instalação das Dependências

```bash
# Usando npm
npm install

# Usando pnpm
pnpm install
