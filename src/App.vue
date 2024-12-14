<template>
  <div class="game-container">
    <line-items :lines='lines' :circleRadius='circleRadius' />
    <circle-group v-for="(circle, index) in circles" :key="index" :active="activeIndex === index" :position="circle"
      @circle-clicked="handleCircleClick(circle)" @change-active="changeActive(index)" />
  </div>
</template>

<script>
import CircleGroup from './components/CircleGroup.vue';
import LineItems from './components/LineItems.vue';

export default {
  components: { CircleGroup, LineItems },

  data() {
    return {
      circles: this.generateCirclePositions(),
      lines: [],
      activeIndex: null,
      circleRadius: 25 // Радиус кружка
    };
  },

  methods: {
    changeActive(index) {
      // Сброс активного индекса, если последняя линия завершена или нет линий
      if (!this.lines.length || this.lines.at(-1)?.end) {
        this.activeIndex = null;
      } else {
        this.activeIndex = index;
      }
    },

    generateCirclePositions() {
      const positions = [];
      const radius = 200; // радиус круга
      const centerX = window.innerWidth / 2;
      const centerY = window.innerHeight / 2;

      for (let i = 1; i <= 10; i++) {
        const angle = (i / 10) * (2 * Math.PI);
        positions.push({
          id: i,
          x: centerX + radius * Math.cos(angle), // центр кружка
          y: centerY + radius * Math.sin(angle) // центр кружка
        });
      }
      return positions;
    },

    handleCircleClick({ id, ...position }) {
      if (this.isNewLine()) {
        this.startNewLine(position, id);
      } else {
        this.toggleLine(position);
      }
    },

    isNewLine() {
      return this.lines.length === 0 || this.lines[this.lines.length - 1]?.end;
    },

    startNewLine(position, id) {
      // Начинаем новую линию
      this.lines.push({ start: position, end: null, id });
    },

    toggleLine(position) {
      const lastLine = this.lines.at(-1);
      const currentLine = { start: lastLine.start, end: position };

      const index = this.findLineIndex(currentLine);

      if (index !== -1) {
        // Если линия уже существует, удаляем её
        this.lines.splice(index, 1);
        // Удаляем последнюю линию, если она не завершена
        if (this.lines.length > 0 && this.lines[this.lines.length - 1].end === null) {
          this.lines.pop();
        }
      } else {
        // Завершаем текущую линию
        this.lines[this.lines.length - 1].end = position;
      }
    },

    findLineIndex(currentLine) {
      return this.lines.findIndex(({ start, end }) =>
        this.arePointsEqual(start, currentLine.start) && this.arePointsEqual(end, currentLine.end) ||
        this.arePointsEqual(start, currentLine.end) && this.arePointsEqual(end, currentLine.start)
      );
    },

    arePointsEqual(pointA, pointB) {
      return pointA?.x === pointB?.x && pointA?.y === pointB?.y;
    }
  },
}
</script>

<style>
.game-container {
  position: relative;
}
</style>
