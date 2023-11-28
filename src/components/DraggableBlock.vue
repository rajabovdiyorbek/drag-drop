<template>
<div class="container">
  <div
      v-for="(element, index) in myArray"
      :key="element.id"
      class="card"
      :draggable="true"
      @dragstart="dragStart($event, index)"
      @dragend="dragEnd($event, index)"
      @dragover="dragOver($event, index)"
      @drop="drop($event, index)"
    >
      <div>
        <img :src="element.photo" :alt="element.photo" class="card--image" />
        <span class="card--number">{{ index + 1 }}</span>
        <img class="card--more" src="../assets/icons/more.svg">
      </div>
    </div>
    <div class="card--image__add">
      <img class="plus" src="../assets/icons/gray-plus.svg">
      <span class="text">Добавить фото</span>
  </div>
</div>
</template>
<script>

export default {
  name: 'DraggableBlock',

  data() {
    return {
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
      dragIndex: null,
    };
  },
  methods: {
    dragStart(event, index) {
      event.target.style.opacity = '0.4'
      // setTimeout(function(){
      //   event.currentTarget.classList.remove("dragging");
      // }, 1);
      this.dragIndex = index;
    },
    dragEnd(event) {
      event.target.style.opacity = '1'
    },
    dragOver (event, index) {
      console.log(index === this.dragIndex)
    },
    drop(event, index) {
      if (this.dragIndex !== null) {
        const draggedElement = this.myArray[this.dragIndex];
        this.myArray.splice(this.dragIndex, 1);
        this.myArray.splice(index, 0, draggedElement);
        this.dragIndex = null;
        this.dragOverIndex = null;
      }
    },
  }
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
  cursor: pointer;
}
.card:hover {
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
.dragging {
  border: 1px solid #FF1919 !important;
}

.sortable-chosen .card--more {
  display: none;
}
.sortable-chosen.sortable-ghost {
  border: none !important;
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
