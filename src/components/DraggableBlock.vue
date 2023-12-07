<template>
  <div class="container">
    <draggable
      v-model="myArray"
      v-bind="dragOptions"
      :move="onMove"
      @start="onDragStart"
      @end="onDragEnd"
    >
      <div
        v-for="(element, index) in myArray"
        :key="element.id"
        class="cards"
        @dragleave="dragLeave"
        @dragover="dragOver"
      >
        <div v-if="isDragging && index === draggedIndex" class="ghost">
          <span class="card--number">{{ draggedOrder }}</span>
          <svg
            width="16"
            height="16"
            viewBox="0 0 16 16"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <g id="icon" clip-path="url(#clip0_43_6815)">
              <path
                id="Icon"
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M8 -0.5C8.55229 -0.5 9 -0.0522848 9 0.5V7H15.5C16.0523 7 16.5 7.44772 16.5 8C16.5 8.55229 16.0523 9 15.5 9H9V15.5C9 16.0523 8.55229 16.5 8 16.5C7.44772 16.5 7 16.0523 7 15.5V9H0.5C-0.0522848 9 -0.5 8.55229 -0.5 8C-0.5 7.44772 -0.0522848 7 0.5 7H7V0.5C7 -0.0522848 7.44772 -0.5 8 -0.5Z"
                fill="#FF1919"
              />
            </g>
            <defs>
              <clipPath id="clip0_43_6815">
                <rect width="16" height="16" fill="white" />
              </clipPath>
            </defs>
          </svg>
        </div>
        <div class="card">
          <img :src="element.photo" :alt="element.photo" class="card--image" />
          <span class="card--number">{{ element.order }}</span>
          <img
            v-if="!isChosen"
            class="card--more"
            src="../assets/icons/more.svg"
          />
        </div>
      </div>
    </draggable>
    <div class="card--image__add">
      <svg
        width="16"
        height="16"
        viewBox="0 0 16 16"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <g id="icon" clip-path="url(#clip0_43_6815)">
          <path
            id="Icon"
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M8 -0.5C8.55229 -0.5 9 -0.0522848 9 0.5V7H15.5C16.0523 7 16.5 7.44772 16.5 8C16.5 8.55229 16.0523 9 15.5 9H9V15.5C9 16.0523 8.55229 16.5 8 16.5C7.44772 16.5 7 16.0523 7 15.5V9H0.5C-0.0522848 9 -0.5 8.55229 -0.5 8C-0.5 7.44772 -0.0522848 7 0.5 7H7V0.5C7 -0.0522848 7.44772 -0.5 8 -0.5Z"
            fill="#C4C9D4"
          />
        </g>
        <defs>
          <clipPath id="clip0_43_6815">
            <rect width="16" height="16" fill="white" />
          </clipPath>
        </defs>
      </svg>
      <span class="text">Добавить фото</span>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  name: "DraggableBlock",
  components: {
    draggable,
  },
  data() {
    return {
      startY: 0,
      myArray: [
        { id: 1, order: 1, photo: require("@/assets/img1.jpeg") },
        { id: 2, order: 2,  photo: require("@/assets/img2.jpeg") },
        { id: 3, order: 3,  photo: require("@/assets/img3.jpeg") },
        { id: 4, order: 4,  photo: require("@/assets/img4.jpeg") },
        { id: 5, order: 5,  photo: require("@/assets/img5.jpeg") },
        { id: 6, order: 6,  photo: require("@/assets/img6.jpeg") },
      ],
      dragOptions: {
        animation: 150,
      },
      isDragging: false,
      isChosen: false,
      draggedIndex: -1,
      draggedOrder: 1,
    };
  },
  methods: {
    onDragStart(evt) {
      this.isChosen = true;
      this.draggedIndex = evt.oldIndex;
      this.startY = evt.originalEvent.clientY;
      this.draggedOrder = evt.oldIndex + 1
    },
    onDragEnd() {
      this.isDragging = false;
      this.isChosen = false;
      this.draggedIndex = -1;
      console.log(this.myArray);
    },
    onMove(evt) {
      const draggedIndex = evt.draggedContext.index;
      const newIndex = evt.draggedContext.futureIndex;
      const movedElement = this.myArray[draggedIndex];
      const startOrder = movedElement.order;
      const endOrder = newIndex + 1;

      this.myArray.forEach((item) => {
        const itemOrder = item.order;

        if (item !== movedElement) {
          const isBetweenOrders = startOrder < endOrder
            ? itemOrder > startOrder && itemOrder <= endOrder
            : itemOrder < startOrder && itemOrder >= endOrder;

          if (isBetweenOrders) {
            item.order = startOrder < endOrder ? itemOrder - 1 : itemOrder + 1;
          }
        }
      });

      movedElement.order = endOrder;
      this.draggedOrder = endOrder;
},
      dragOver(evt) {
      if (evt.clientY - this.startY > 40) {
        this.isDragging = true;
      }
    },
    dragLeave(evt) {
      if (
        evt.relatedTarget === null ||
        (evt.currentTarget && !evt.currentTarget.contains(evt.relatedTarget))
      ) {
        this.isDragging = true;
      }
    },
  },
};
</script>

<style lang="scss">
.container {
  display: flex;
}
.cards {
  position: relative;
  border: 1px solid #c4c9d4;
  margin: 1px;
  width: 108px;
  height: 144px;
  margin-right: 8px;
  border-radius: 4px;
  display: inline-block;
  text-align: center;
  background: #fff;
  cursor: pointer;

  &:hover {
    border: 1px solid #578fc7;
  }
}

.card {
  &--image {
    width: 100%;
    height: 144px;
    border-radius: 4px;
    object-fit: cover;

    &__add {
      background: white;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      width: 108px;
      height: 144px;
      border-radius: 4px;
      border: 1px dashed #c4c9d4;
      cursor: pointer;

      &:hover {
        border-color: #578fc7;
        color: #578fc7;
      }

      &:hover svg path {
        fill: #578fc7;
      }
    }
  }

  &--number {
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
  &--more {
    position: absolute;
    width: 20px;
    top: 5px;
    right: 5px;
    background: white;
    padding: 2px;
    font-size: 14px;
    cursor: pointer;
  }
}
.text {
  text-align: center;
  width: 60px;
  font-size: 13px;
  font-weight: 500;
}
.plus {
  width: 14px;
}
.sortable-chosen {
  border: 1px solid #ff1919 !important;

  & .card--more {
    display: none;
  }

  & .card--number {
    display: none;
  }

  &.sortable-ghost {
    transform: none !important;
    border: none !important;
    transition: none;

    .card {
      opacity: 0.4;
    }

    .card--number {
      display: block;
    }
  }
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
  border: 1px dashed #ff1919;
  z-index: 3;
}
</style>
