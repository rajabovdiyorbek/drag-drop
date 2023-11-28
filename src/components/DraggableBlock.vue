<template>
<div class="container">
    <draggable v-model="myArray" v-bind="dragOptions" @start="onDragStart"  @end="onDragEnd">
      <div
        v-for="(element, index) in myArray"
        :key="element.id"
        class="cards"
        @dragenter="dragEnter"
        @dragleave="dragLeave"
        @dragover="dragOver"
      >
        <div v-if="isDragging && index === draggedIndex" class="ghost">
            <span class="card--number">{{ index + 1 }}</span>
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g id="icon" clip-path="url(#clip0_43_6815)">
            <path id="Icon" fill-rule="evenodd" clip-rule="evenodd" d="M8 -0.5C8.55229 -0.5 9 -0.0522848 9 0.5V7H15.5C16.0523 7 16.5 7.44772 16.5 8C16.5 8.55229 16.0523 9 15.5 9H9V15.5C9 16.0523 8.55229 16.5 8 16.5C7.44772 16.5 7 16.0523 7 15.5V9H0.5C-0.0522848 9 -0.5 8.55229 -0.5 8C-0.5 7.44772 -0.0522848 7 0.5 7H7V0.5C7 -0.0522848 7.44772 -0.5 8 -0.5Z" fill="#FF1919"/>
          </g>
          <defs>
            <clipPath id="clip0_43_6815">
              <rect width="16" height="16" fill="white"/>
            </clipPath>
          </defs>
        </svg>
        </div>
        <div class="card">
          <img :src="element.photo" :alt="element.photo" class="card--image" />
          <span class="card--number">{{ index + 1 }}</span>
          <img v-if="!isChosen" class="card--more" src="../assets/icons/more.svg">
        </div>
      </div>
  </draggable>
  <div class="card--image__add">
    <svg  width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
  <g id="icon" clip-path="url(#clip0_43_6815)">
    <path id="Icon" fill-rule="evenodd" clip-rule="evenodd" d="M8 -0.5C8.55229 -0.5 9 -0.0522848 9 0.5V7H15.5C16.0523 7 16.5 7.44772 16.5 8C16.5 8.55229 16.0523 9 15.5 9H9V15.5C9 16.0523 8.55229 16.5 8 16.5C7.44772 16.5 7 16.0523 7 15.5V9H0.5C-0.0522848 9 -0.5 8.55229 -0.5 8C-0.5 7.44772 -0.0522848 7 0.5 7H7V0.5C7 -0.0522848 7.44772 -0.5 8 -0.5Z" fill="#C4C9D4"/>
  </g>
  <defs>
    <clipPath id="clip0_43_6815">
      <rect width="16" height="16" fill="white"/>
    </clipPath>
  </defs>
</svg>
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
      startY: 0,
      myArray: [
        { id: 1, photo: require('@/assets/img1.jpeg') },
        { id: 2, photo: require('@/assets/img2.jpeg') },
        { id: 3, photo: require('@/assets/img3.jpeg') },
        { id: 7, photo:require('@/assets/img6.jpeg') },
        { id: 4, photo: require('@/assets/img4.jpeg') },
        { id: 8, photo: require('@/assets/img1.jpeg') },
        { id: 5, photo: require('@/assets/img5.jpeg') },
        { id: 6, photo:require('@/assets/img6.jpeg') },
        { id: 9, photo: require('@/assets/img1.jpeg') },

      ],
      dragOptions: {
        swapThreshold: 1,
        animation: 150,
      },
      isDragging: false,
      isChosen: false,
      draggedIndex: -1,
    };
  },
  methods: {
    onDragStart(evt) {
      this.isChosen = true
      this.draggedIndex = evt.oldIndex;
      this.startY = evt.originalEvent.clientY
    },
    onDragEnd() {
      this.isDragging = false;
      this.isChosen = false
      this.draggedIndex = -1;
    },
    dragEnter(evt) {
      if (evt.relatedTarget && !evt.currentTarget.contains(evt.relatedTarget)) {
        this.isDragging = true;
      }
    },
    dragOver(evt) {
      if (evt.clientY - this.startY > 40) {
        this.isDragging = true;
      }
    },
    dragLeave(evt) {
      if (evt.relatedTarget === null || (evt.currentTarget && !evt.currentTarget.contains(evt.relatedTarget))) {
        this.isDragging = true;
      }
    },
  },
};
</script>

<style>
.container {
  display: flex;
}
.cards {
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
  cursor: pointer;
}
.cards:hover {
  border: 1px solid #578FC7;
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

.card--image__add:hover {
  border-color: #578FC7;
  color: #578FC7;
}

.card--image__add:hover svg path {
  fill: #578FC7;
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
  width: 20px;
  top: 5px;
  right: 5px;
  background: white;
  padding: 2px;
  font-size: 14px;
  cursor: pointer;
}
.sortable-chosen .card--more {
  display: none;
}
.sortable-chosen {
  border: 1px solid #FF1919 !important;
}

.sortable-chosen .card--number {
  display: none;
}
.sortable-chosen.sortable-ghost {
  transform: none !important;
  border: none !important;
  transition: none;
}

.sortable-chosen.sortable-ghost .card {
  opacity: 0.4;
}
.sortable-chosen.sortable-ghost .card--number {
  display: block;
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
  z-index: 3;
}

</style>
