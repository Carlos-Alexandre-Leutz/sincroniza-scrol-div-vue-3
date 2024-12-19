<template>
  <div>
    <!-- Div 1 -->
    <div
      :ref="(el) => (refs[`divScrolx1`] = el)"
      class="scrollable"
      @scroll="syncScroll('divScrolx1')"
    >
      <div class="content">
        <p v-for="n in 20" :key="n">Conteúdo da Div 1 - Linha {{ n }}</p>
      </div>
    </div>

    <!-- Div 2 -->
    <div
      :ref="(el) => (refs[`divScrolx2`] = el)"
      class="scrollable"
      @scroll="syncScroll('divScrolx2')"
    >
      <div class="content">
        <p v-for="n in 20" :key="n">Conteúdo da Div 2 - Linha {{ n }}</p>
      </div>
    </div>

    <!-- Div 3 gerada dinamicamente -->
    <div v-for="(item, index) in div3Items" :key="item.label">
      <div
        :ref="(el) => (refs[`divScrolx3-${index}`] = el)"
        class="scrollable"
        @scroll="syncScroll(`divScrolx3-${index}`)"
      >
        <div class="content">
          <p v-for="n in 20" :key="n">Conteúdo da Div 3 - Linha {{ n }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from "vue";
export default {
  setup() {
    const isSyncing = ref(false);
    const div3Items = ref([{ label: "A" }, { label: "B" }, { label: "C" }]);
    const refs = reactive({}); // Gerencia referências dinamicamente

    const syncScroll = (sourceRef) => {
      if (isSyncing.value) return;
      isSyncing.value = true;

      const source = refs[sourceRef]; // Busca a referência do elemento fonte
      if (!source) {
        isSyncing.value = false;
        return;
      }

      const { scrollLeft } = source;

      // Atualiza a posição de scroll em todos os elementos, exceto no de origem
      Object.values(refs).forEach((div) => {
        if (div !== source) {
          div.scrollLeft = scrollLeft;
        }
      });

      isSyncing.value = false;
    };

    return {
      div3Items,
      refs,
      syncScroll,
    };
  },
};
</script>

<style>
.scrollable {
  width: 300px;
  height: 100px;
  overflow-x: auto;
  overflow-y: hidden;
  border: 1px solid #ccc;
  margin-bottom: 10px;
  white-space: nowrap;
}

.content {
  width: 1000px;
}
</style>
