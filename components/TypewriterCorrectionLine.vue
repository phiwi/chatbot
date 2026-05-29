<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

const props = withDefaults(
  defineProps<{
    prefix: string
    wrongWord: string
    correctWord: string
    suffix?: string
    lineClass?: string
    startDelay?: number
    typeMs?: number
    backspaceMs?: number
    pauseOnErrorMs?: number
    pauseAfterFixMs?: number
    holdDoneMs?: number
    loopPauseMs?: number
    wordPauseChance?: number
    wordPauseMinMs?: number
    wordPauseMaxMs?: number
  }>(),
  {
    suffix: '',
    lineClass: '',
    startDelay: 0,
    typeMs: 24,
    backspaceMs: 34,
    pauseOnErrorMs: 650,
    pauseAfterFixMs: 240,
    holdDoneMs: 1500,
    loopPauseMs: 450,
    wordPauseChance: 0.26,
    wordPauseMinMs: 35,
    wordPauseMaxMs: 130,
  },
)

const displayText = ref('')
let stopped = false
const timers: ReturnType<typeof setTimeout>[] = []

function isWordChar(char: string) {
  return /[A-Za-z0-9]/.test(char)
}

function randomBetween(min: number, max: number) {
  return Math.round(min + Math.random() * Math.max(0, max - min))
}

function shouldPauseAfterWord(target: string, index: number) {
  const current = target[index - 1] ?? ''
  const next = target[index] ?? ''
  return isWordChar(current) && (!next || !isWordChar(next))
}

function wait(ms: number) {
  return new Promise<void>((resolve) => {
    const timer = setTimeout(resolve, ms)
    timers.push(timer)
  })
}

async function typeTo(target: string, speedMs: number) {
  for (let i = displayText.value.length + 1; i <= target.length; i += 1) {
    if (stopped) return
    displayText.value = target.slice(0, i)
    await wait(speedMs)

    if (
      shouldPauseAfterWord(target, i)
      && Math.random() < props.wordPauseChance
    ) {
      await wait(randomBetween(props.wordPauseMinMs, props.wordPauseMaxMs))
    }
  }
}

async function deleteToLength(targetLength: number, speedMs: number) {
  while (displayText.value.length > targetLength) {
    if (stopped) return
    displayText.value = displayText.value.slice(0, -1)
    await wait(speedMs)
  }
}

async function runLoop() {
  const wrongTarget = `${props.prefix}${props.wrongWord}`
  const fixedTarget = `${props.prefix}${props.correctWord}`
  const doneTarget = `${fixedTarget}${props.suffix}`

  if (props.startDelay > 0) {
    await wait(props.startDelay)
  }

  while (!stopped) {
    displayText.value = ''

    await typeTo(wrongTarget, props.typeMs)
    await wait(props.pauseOnErrorMs)

    await deleteToLength(props.prefix.length, props.backspaceMs)
    await wait(props.pauseAfterFixMs)

    await typeTo(fixedTarget, props.typeMs)
    await typeTo(doneTarget, props.typeMs)

    await wait(props.holdDoneMs)

    await deleteToLength(0, Math.max(14, Math.round(props.backspaceMs * 0.72)))
    await wait(props.loopPauseMs)
  }
}

onMounted(() => {
  void runLoop()
})

onBeforeUnmount(() => {
  stopped = true
  for (const timer of timers) {
    clearTimeout(timer)
  }
})
</script>

<template>
  <span class="chat-line dynamic" :class="lineClass">{{ displayText }}</span>
</template>
