<template>
<div class="container">
    <draggable v-model="myArray" :options="dragOptions" @start="onDragStart" @end="onDragEnd">
      <div
        v-for="(element, index) in myArray"
        :key="element.id"
        class="card"
      >
        <div v-if="isDragging && index === draggedIndex" class="ghost">
            <span class="card--number">{{ index + 1 }}</span>
            <img src="../assets/icons/plus.svg" alt="plus-icon" />
        </div>
        <div v-else @dragleave="dragEnter">
          <img :src="element.photo" :alt="element.photo" class="card--image" />
          <span class="card--number">{{ index + 1 }}</span>
          <img v-if="!isDragging" class="card--more" src="../assets/icons/more.svg">
        </div>
      </div>
  </draggable>
  <div class="card--image__add">
      <img class="plus" src="../assets/icons/gray-plus.svg">
      <span class="text">Добавить фото</span>
  </div>
</div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  name: 'DraggableBlock',
  components: {
    draggable,
  },
  data() {
    return {
      myArray: [
        { id: 1, photo: require('@/assets/img1.jpeg') },
        { id: 2, photo: require('@/assets/img2.jpeg') },
        { id: 3, photo: require('@/assets/img3.jpeg') },
        { id: 4, photo: require('@/assets/img4.jpeg') },
        { id: 5, photo: require('@/assets/img5.jpeg') },
        { id: 6, photo:require('@/assets/img6.jpeg') },
      ],
      dragOptions: {
      },
      selectedCard: null,
      isDragging: false,
      isChosen: false,
      draggedIndex: -1,
    };
  },
  methods: {
    onDragStart(evt) {
      this.draggedIndex = evt.oldIndex;
    },
    onDragEnd() {
      this.isDragging = false;
      this.draggedIndex = -1;
    },
    dragEnter() {
      this.isDragging = true;
    },
  },
};
</script>

<style>
.container {
  display: flex;
}
.card {
  position: relative;
  border: 1px solid #C4C9D4;
  margin: 1px;
  width: 108px;
  height: 144px;
  margin-right: 8px;
  border-radius: 4px;
  display: inline-block;
  text-align: center;
  background: #fff;
}


.card--image {
  width: 100%;
  height: 144px;
  border-radius: 4px;
  object-fit: cover;
}
.card--image__add {
  background: white;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 108px;
  height: 144px;
  border-radius: 4px;
  border: 1px dashed #C4C9D4;
  cursor: pointer;
}

.text {
  text-align: center;
  width: 60px ;
  font-size: 13px;
  font-weight: 500;
}

.plus {
  width: 14px;
}

.card--number {
  position: absolute;
  width: 16px;
  top: 5px;
  left: 5px;
  background: #f2f4f7;
  color: #2a2b33;
  padding: 4px;
  border-radius: 50%;
  font-size: 14px;
}
.card--more {
  position: absolute;
  width: 16px;
  top: 5px;
  right: 5px;
  background: white;
  padding: 4px;
  font-size: 14px;
  cursor: pointer;
}
.sortable-chosen {
  border: 1px solid #FF1919;
}
.sortable-chosen .card--number {
  display: none;
}
.sortable-chosen .card--more {
  display: none;
}
.sortable-chosen.sortable-ghost {
  border: none;
  transition: none;
}

.ghost {
  background: white;
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 108px;
  height: 144px;
  border-radius: 4px;
  border: 1px dashed #FF1919;
}

</style>
