<script setup lang="ts">
const imagens = [
  '/img/background.png',
  '/img/toalha_mesa.png',
  '/img/toalha_mesa2.png',
  '/img/moldura_3.png',
  '/img/manequim.jpeg',
  '/img/manequim2.png',
  '/img/telheiro.png',
  '/img/envelope.png',
]

const carregadas = ref(0)
const pronto = ref(false)
const progresso = computed(() => Math.round((carregadas.value / imagens.length) * 100))

// Bloqueia o scroll enquanto carrega
useHead({
  bodyAttrs: { class: computed(() => (pronto.value ? '' : 'overflow-hidden')) },
})

onMounted(async () => {
  await Promise.all(
    imagens.map(
      src =>
        new Promise<void>((resolve) => {
          const img = new Image()
          const fim = () => {
            carregadas.value++
            resolve()
          }
          img.onload = fim
          img.onerror = fim // se falhar, não bloqueia o site
          img.src = src
        }),
    ),
  )

  // Fontes
  await Promise.all([
    document.fonts.load("1em 'Abramo Script'"),
    document.fonts.load("1em '29LT Zarid Display'"),
  ]).catch(() => { })

  setTimeout(() => (pronto.value = true), 300)
})

const evento = new Date('2026-11-15T19:00:00')

const dias = ref(0)
const horas = ref(0)
const minutos = ref(0)

function atualizar() {
  const diff = Math.max(evento.getTime() - Date.now(), 0)
  dias.value = Math.floor(diff / 86_400_000)
  horas.value = Math.floor((diff % 86_400_000) / 3_600_000)
  minutos.value = Math.floor((diff % 3_600_000) / 60_000)
}

let timer: ReturnType<typeof setInterval>
onMounted(() => {
  atualizar()
  timer = setInterval(atualizar, 30_000)
})
onBeforeUnmount(() => clearInterval(timer))
</script>

<template>
  <section class="relative h-dvh w-full overflow-hidden [--s:min(calc(100vw/390),calc(100dvh/844))]">
    <img src="/img/background.png" alt="Background" class="absolute inset-0 h-full w-full object-cover">

    <div class="absolute left-1/2 top-1/2 z-10 h-[70vh] w-[97vw] -translate-x-1/2 -translate-y-1/2">
      <img src="/img/toalha_mesa.png" alt="Toalha Mesa" class="h-full w-full scale-110 object-contain">

      <div class="absolute left-1/2 top-[calc(var(--s)*90)] z-20 w-[15%] -translate-x-1/2">
        <img src="/img/moldura_3.png" alt="Moldura" class="block w-full opacity-30">

        <p
          class="font-abramo-script absolute left-1/2 top-[calc(var(--s)*120)] z-20 ml-[calc(var(--s)*20)] w-[calc(var(--s)*240)] -translate-x-1/2 rotate-340 text-left text-[length:calc(var(--s)*100)] leading-[calc(var(--s)*36)] text-taupe-600">
          how lucky <br> we are
        </p>

        <div
          class="absolute left-1/2 top-[calc(var(--s)*172)] z-10 h-[calc(var(--s)*121)] w-[calc(var(--s)*82)] -translate-x-1/2 bg-orange-100">
          <img src="/img/manequim.jpeg" alt="Manequim"
            class="ml-[calc(var(--s)*5)] mt-[calc(var(--s)*5)] h-[calc(var(--s)*96)] w-[calc(var(--s)*72)] object-cover">
          <span
            class="font-zarid absolute -bottom-[calc(var(--s)*4)] right-[calc(var(--s)*8)] text-[length:calc(var(--s)*16)]">27</span>
        </div>

        <span
          class="font-zarid absolute left-1/2 top-[calc(var(--s)*315)] z-20 block w-[calc(var(--s)*270)] -translate-x-1/2 text-center text-[length:calc(var(--s)*12)] leading-[calc(var(--s)*18)] text-taupe-600">
          Twenty-seven years of little moments, unexpected turns, beautiful memories
          and people who made it all a little more special.
          And somehow, when I look back, the best part was never the years themselves.
          It was the people I got to share them with.
        </span>
      </div>
    </div>
  </section>

  <!-- ========== SECTION 2: CONVITE ========== -->
  <section id="convite"
    class="relative h-dvh w-full overflow-hidden bg-[#f6f3e9] [--s:min(calc(100vw/390),calc(100dvh/844))]">
    <div class="absolute inset-x-0 top-0 h-[15%] overflow-hidden">
      <img src="/img/background.png" alt="Background"
        class="h-dvh w-full max-w-none rotate-180 -scale-x-100 object-cover">
    </div>
    <img src="/img/telheiro.png" alt="Telheiro" class="absolute inset-x-0 top-[15%] h-[29%] w-full object-cover">

    <!-- Toalha com título e informações -->
    <div class="@container absolute left-1/2 top-[3%] z-10 w-[36%] -translate-x-1/2">
      <img src="/img/toalha_mesa2.png" alt="Toalha Mesa" class="block w-full">

      <p
        class="font-abramo-script absolute left-1/2 top-[28%] w-full -translate-x-1/2 -translate-y-1/2 whitespace-nowrap text-center text-[length:10cqw] text-taupe-500">
        how lucky we are
      </p>

      <div
        class="font-zarid absolute left-1/2 top-[75%] w-full -translate-x-1/2 -translate-y-1/2 text-center text-taupe-500">
        <p class="text-[length:5cqw] leading-[1.5]">
          15 Novembro<br>19H00<br>O Telheiro
        </p>
        <p class="mt-[2cqw] text-[length:2.8cqw]">[MORADA]</p>
      </div>
    </div>

    <!-- Envelope -->
    <img src="/img/envelope.png" alt="Envelope"
      class="absolute left-1/2 top-[44%] z-10 w-[40%] -translate-x-1/2 -translate-y-[63%]">

    <!-- Texto -->
    <div
      class="font-zarid absolute inset-x-0 bottom-0 top-[51%] flex flex-col items-center justify-between px-[5%] pb-[3%] text-center text-[length:calc(var(--s)*9)] leading-[calc(var(--s)*13)] text-taupe-500">
      <p>
        Há momentos em que paramos por um instante e percebemos a sorte que temos.<br>
        Não porque a vida tenha sido sempre perfeita, nem porque todos os caminhos tenham sido fáceis.<br>
        Mas porque, no meio de tudo, dias bons, dias menos bons, mudanças e das voltas que a vida dá, houve sempre
        alguém para tornar o caminho mais bonito.<br>
        Pessoas que ficaram.<br>
        Pessoas com quem partilhámos conversas que ficaram pela noite dentro.<br>
        Pessoas que, sem sequer saberem, transformaram momentos simples em memórias que vamos guardar para sempre.
      </p>

      <p class="font-abramo-script text-[length:calc(var(--s)*20)] leading-none">E tu és uma dessas pessoas</p>

      <p>
        Por isso, ao chegar aos 27, não quero celebrar apenas mais um ano.<br>
        Quero celebrar tudo o que trouxe até aqui.<br>
        As histórias que ficaram, as que ainda estão por contar e, sobretudo, as pessoas que fazem parte delas.<br>
        Quero juntar à mesa algumas das minhas pessoas favoritas, ouvir música, rir sem horas, brindar às pequenas
        coisas e criar mais memórias que, daqui a muitos anos, ainda nos vão fazer sorrir.<br>
        Porque, no fim, talvez seja isso que torna a vida tão bonita.<br>
        Não são os anos que contamos.<br>
        São as pessoas com quem os vivemos.<br>
        E se há coisa que estes 27 anos me ensinaram,<br>
        é que tive, e tenho, muita sorte.
      </p>

      <p>Que sorte a minha ter-vos na minha vida.</p>

      <p class="font-abramo-script text-[length:calc(var(--s)*28)] leading-none">how lucky we are!</p>

      <a href="#menu" aria-label="Seguinte">
        <svg class="h-[calc(var(--s)*20)] w-[calc(var(--s)*20)]" viewBox="0 0 24 24" fill="none" stroke="currentColor"
          stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
          <path d="m6 9 6 6 6-6" />
        </svg>
      </a>
    </div>
  </section>

  <!-- ========== SECTION 3: MENU + CONTAGEM ========== -->
  <section id="menu" class="relative h-dvh w-full overflow-hidden [--s:min(calc(100vw/390),calc(100dvh/844))]">
    <img src="/img/background.png" alt="Background" class="absolute inset-x-0 top-0 h-[46%] w-full object-cover">
    <img src="/img/manequim2.png" alt="Manequim" class="absolute inset-x-0 bottom-0 h-[54%] w-full object-cover">
    <div class="absolute inset-x-0 bottom-0 h-[54%] bg-black/10"></div>

    <!-- Toalha com menu -->
    <div class="@container absolute left-1/2 top-[23%] z-10 w-[50%] -translate-x-1/2 -translate-y-1/2">
      <img src="/img/toalha_mesa.png" alt="Toalha Mesa" class="block w-full">

      <div
        class="font-zarid absolute left-1/2 top-[18%] w-[74%] -translate-x-1/2 text-center text-[length:2.7cqw] uppercase leading-[1.35] text-taupe-500">
        <p class="mb-[3cqw]">35 €</p>

        <p class="italic">Couvert:</p>
        <p class="mb-[3cqw]">Pão azeitonas manteiga aromatizada</p>

        <p class="italic">Entradas:</p>
        <p>Bolo do caco</p>
        <p>Tábua de queijos e enchidos</p>
        <p>Salada grega</p>
        <p>Camarão folhado</p>
        <p>Paté de atum</p>
        <p class="mb-[5cqw]">Salgadinhos</p>

        <p class="mb-[3cqw] italic">2 pratos principais</p>

        <p class="mb-[5cqw] italic">Sobremesas</p>

        <p class="italic">Bebidas:</p>
        <p>Vinhos da casa</p>
        <p>Cerveja</p>
        <p>Refrigerantes</p>
        <p>Água natural/gaseificada</p>
        <p class="mb-[3cqw]">Limonada</p>

        <p>Café</p>
      </div>
    </div>

    <!-- Contagem decrescente -->
    <div
      class="font-zarid absolute left-1/2 top-[70.5%] z-10 grid w-[54%] -translate-x-1/2 grid-cols-3 text-center text-[#f3ece0]">
      <div>
        <p class="text-[length:calc(var(--s)*40)] leading-none">{{ dias }}</p>
        <p class="mt-[calc(var(--s)*4)] text-[length:calc(var(--s)*10)] uppercase tracking-wider">Dias</p>
      </div>
      <div>
        <p class="text-[length:calc(var(--s)*40)] leading-none">{{ horas }}</p>
        <p class="mt-[calc(var(--s)*4)] text-[length:calc(var(--s)*10)] uppercase tracking-wider">Horas</p>
      </div>
      <div>
        <p class="text-[length:calc(var(--s)*40)] leading-none">{{ minutos }}</p>
        <p class="mt-[calc(var(--s)*4)] text-[length:calc(var(--s)*10)] uppercase tracking-wider">Minutos</p>
      </div>
    </div>

    <p
      class="font-abramo-script absolute inset-x-0 top-[86%] z-10 text-center text-[length:calc(var(--s)*22)] leading-[1.4] text-[#f3ece0]">
      Aos 27, à vida<br>e às pessoas que a tornam mais bonita.
    </p>
  </section>
</template>