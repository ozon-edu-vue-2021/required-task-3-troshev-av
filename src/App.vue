<template>
  <div id="app">
    <div class="office">
      <Map :legend="legend" :tables="tables" @select="selectTable($event)" />
      <SideMenu
        :legend.sync="legend"
        :person="opennedPerson"
        :is-user-openned="isUserOpenned"
        @update:is-user-openned="unselectTable()"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";

import legendData from "@/assets/data/legend.json";
import tablesData from "@/assets/data/tables.json";
import peopleData from "@/assets/data/people.json";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data() {
    const legend = legendData.map((item) => {
      const tables = tablesData.filter(
        ({ group_id }) => group_id === item.group_id
      );

      return {
        ...item,
        counter: tables.length,
      };
    });

    return {
      tables: tablesData,
      legend,
      selectedTableId: null,
    };
  },
  computed: {
    opennedPerson() {
      if (this.selectedTableId == null) return null;

      const people = this._getPersonByTableId(this.selectedTableId);
      const group = this._getGroupByTableId(this.selectedTableId);
      return {
        ...people,
        group: group.text,
      };
    },
    isUserOpenned() {
      return Boolean(this.opennedPerson);
    },
  },
  methods: {
    _getPersonByTableId(tableId) {
      return peopleData.find((item) => item.tableId === tableId);
    },
    _getTableById(tableId) {
      return this.tables.find((item) => item._id === tableId);
    },
    _getGroupByTableId(tableId) {
      const table = this._getTableById(tableId);
      const groupId = table.group_id;

      return this.legend.find((item) => item.group_id === groupId);
    },
    selectTable(id) {
      this.selectedTableId = id;
    },
    unselectTable() {
      this.selectedTableId = null;
    },
  },
};
</script>

<style>
#app {
  --primary-hsl: 219deg, 100%, 50%;
  --gray-hsl: 240deg, 14%, 70%;

  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

* {
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

.office {
  display: grid;
  grid-template-columns: 1fr 480px;
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  height: 100%;
  background: white;
  max-width: 1500px;
  margin: 0 auto;
}
</style>
