<script setup lang="ts">
import { useGSAP } from '#imports'

const tableContainerRef = useTemplateRef('tableContainerRef')
const pinnedContentRef = useTemplateRef('pinnedContentRef')

const marginLeft = ref<undefined | string>(undefined)
const dividerLength = ref(15)

onMounted(() => {

  marginLeft.value = window.getComputedStyle(document.querySelector('#ready-to-discuss')).marginLeft

  const rect = tableContainerRef.value.children[1].getBoundingClientRect()

  tableContainerRef.value.children[0].style.width = rect.width + 'px'
  tableContainerRef.value.children[0].style.height = rect.height + 'px'

  tableContainerRef.value.children[2].style.width = rect.width + 'px'
  tableContainerRef.value.children[3].style.width = rect.width + 'px'

  const scrollDelta = tableContainerRef.value.scrollWidth - tableContainerRef.value.clientWidth

  useGSAP().timeline({
    scrollTrigger: {
      trigger: pinnedContentRef.value,
      pin: true,
      start: 'center center',
      end: '+=1000',
      onUpdate: (self) => {
        tableContainerRef.value.scrollLeft = self.progress.toFixed(3) * scrollDelta
      }
    }
  })
})
</script>

<template>
  <section id="roadmap" class="roadmap" :style="{ 'margin-left': marginLeft }">
    <div ref="pinnedContentRef">
      <h2>
        Дорожная карта
      </h2>
      <div class="roadmap__table-container" ref="tableContainerRef">
        <div class="roadmap__steps">
          <div class="roadmap__step --1">Аналитика и проектирование</div>
          <div class="roadmap__step --2">Дизайн</div>
          <div class="roadmap__step --3">Верстка</div>
          <div class="roadmap__step --4">Программирование и ввод в эксплуатацию</div>
        </div>
        <table>
          <tbody>
          <tr>
            <td v-for="(_, index) in dividerLength">
              <template v-if="index === 0">
                Начало
              </template>
              <template v-else-if="index === 14">
                Сдача
              </template>
              <template v-else>
                {{ index * 2 }} нед.
              </template>
            </td>
          </tr>
          <tr>
            <td v-for="n in dividerLength">
              <div class="roadmap__divider-wrapper">
                <div class="roadmap__divider"/>
              </div>
            </td>
          </tr>
          </tbody>
        </table>
        <div class="roadmap__timeline --top"/>
        <div class="roadmap__timeline"/>
      </div>
    </div>
  </section>
</template>
