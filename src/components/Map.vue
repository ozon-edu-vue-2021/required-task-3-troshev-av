<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root" @click="mapClickHandler($event)">
      <MapSVG ref="map" />
      <TableSVG ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import MapSVG from "@/assets/images/map.svg";
import TableSVG from "@/assets/images/workPlace.svg";
import * as d3 from "d3";

export default {
  components: {
    MapSVG,
    TableSVG,
  },
  props: {
    legend: {
      type: Array,
      default: () => [],
    },
    tables: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      isLoading: false,
      svg: null,
      g: null,
      tableSVG: null,
    };
  },
  mounted() {
    this.isLoading = true;

    this.svg = d3.select(this.$refs.map);
    this.g = this.svg.select("g");
    this.tableSVG = d3.select(this.$refs.table);
    if (this.g) {
      this.drawTables();
    } else {
      alert("SVG is incorrect");
    }

    this.isLoading = false;
  },
  methods: {
    drawTables() {
      // создаем группу для рабочик мест
      const svgTablesGroupPlace = this.g
        .append("g")
        .classed("groupPlaces", true);

      const fillsObj = this.legend.reduce((result, item) => {
        result[item.group_id] = item.color;
        return result;
      }, {});

      this.tables.map((table) => {
        // создает группу для рабочего стола
        const targetSeat = svgTablesGroupPlace
          .append("g")
          .attr(
            "transform",
            `translate(${table.x}, ${table.y}) scale(0.5), rotate(${
              table.rotate || 0
            })`
          )
          .attr("data-id", table._id)
          .classed("workplace", true);

        // устанавливает стол в группу
        targetSeat
          .append("g")
          .attr(
            // устанавливаем цвет подразделения
            "fill",
            fillsObj[table.group_id] || "transparent"
          )
          .html(this.tableSVG.html());
      });
    },
    mapClickHandler(event) {
      const $table = event.target.closest(".workplace");
      const tableId = $table ? +$table.dataset.id : null;

      this.selectWorkplace(tableId);
    },
    selectWorkplace(id) {
      this.$emit("select", id);
    },
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>
