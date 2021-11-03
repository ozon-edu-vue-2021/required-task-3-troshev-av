<template>
  <div class="menu">
    <div class="toolbar">
      <div class="toolbar__header">
        <template v-if="!isUserOpenned">
          <h3>Информация</h3>
        </template>
        <template v-else>
          <div class="action" @click="closeProfile">
            <div class="arrow"></div>
          </div>
          <h3>Профиль</h3>
        </template>
      </div>
      <div class="toolbar__actions"></div>
    </div>
    <div class="content">
      <div v-show="!isUserOpenned" class="legend">
        <div class="legend__data">
          <Draggable
            v-if="legend.length > 0"
            :value="legend"
            @input="updateLegend($event)"
            class="legend__items"
          >
            <LegendItem
              v-for="(item, index) in legend"
              :key="index"
              :color="item.color"
              :text="item.text"
              :counter="item.counter"
              class="legend__item"
            />
          </Draggable>
          <span v-else class="legend--empty"> Список пуст </span>
        </div>
        <div class="legend__chart">
          <PieChart :legend="legend" />
        </div>
      </div>
      <div v-if="isUserOpenned" class="profile">
        <div v-if="!person" class="profile__empty">Место пустое</div>

        <PersonCard v-else :person="person" />
      </div>
    </div>
  </div>
</template>

<script>
import Draggable from "vuedraggable";
import LegendItem from "./SideMenu/LegendItem.vue";
import PersonCard from "./SideMenu/PersonCard.vue";
import PieChart from "./SideMenu/PieChart.vue";

import { ARRAY_PROP_DEFAULT } from "@/utils/consts";

export default {
  props: {
    isUserOpenned: {
      type: Boolean,
      default: false,
    },
    person: {
      type: Object,
      default: null,
    },
    legend: {
      type: Array,
      default: ARRAY_PROP_DEFAULT,
    },
  },
  components: {
    Draggable,
    LegendItem,
    PersonCard,
    PieChart,
  },
  methods: {
    closeProfile() {
      this.$emit("update:is-user-openned", false);
    },
    updateLegend(value) {
      this.$emit("update:legend", value);
    },
  },
};
</script>

<style scoped>
.menu {
  border-left: 1px solid #ccd8e4;
  padding: 24px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.toolbar .toolbar__actions button {
  font-size: 0.76rem;
  text-transform: uppercase;
  letter-spacing: 0.08rem;
  padding: 2px 6px;
}

.toolbar__header {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
}

.toolbar__header .action {
  cursor: pointer;
  margin-right: 14px;
  width: 20px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.toolbar__header .action .arrow {
  width: 10px;
  height: 10px;
  border-top: 2px solid blue;
  border-right: 2px solid blue;
  transform: rotate(-135deg);
}

h3 {
  margin: 0;
}

.content {
  flex: 1;
}

.content .legend {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
}

.content .legend .legend__data {
  display: flex;
  height: 100%;
}

.content .legend .legend__items {
  flex: 1;
  width: 100%;
}

.content .legend .legend__items .legend__item:not(:first-child) {
  margin-top: 16px;
}

.content .legend .legend__items .legend__item {
  cursor: pointer;
}

.content .legend .legend__items .legend__item.sortable-chosen {
  opacity: 25%;
}

.content .legend .legend--empty {
  align-self: center;
  width: 100%;
  text-align: center;
}

.profile {
  padding-top: 20px;
}
</style>
