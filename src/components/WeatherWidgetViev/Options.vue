<template :key="regions">
  <div>
    <input
      class="city-input"
      ref="input"
      v-model="name"
      @keydown.enter="addRegion"
      placeholder="Add City"
    />
    <draggable
      :list="regions"
      @start="drag = true"
      @end="drag = false"
      @change="saveRegions"
    >
      <template #item="{ element }">
        <div class="grid-row cards">
          <i class="gear grid-item fa fa-bars" aria-hidden="true"></i>
          <div class="option-name grid-item">{{ element.city }}</div>

          <i
            class="gear grid-item fa fa-trash-o"
            aria-hidden="true"
            @click="deleteRegion(element.id)"
          ></i>
        </div>
      </template>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  data() {
    return {
      drag: false,
    };
  },

  props: ['regions', 'addCustomRegion', 'deleteRegion', 'saveRegions'],

  components: {
    draggable,
  },

  methods: {
    addRegion: function () {
      this.addCustomRegion(this.name);
      this.$refs.input.value = '';
    },
  },
};
</script>

<style>
.city-input {
  height: 30px;
  font-size: 20px;
  width: 100%;
  border: solid;
  color: #00487c;
}
.option-name {
  font-size: 20px;
}
</style>
