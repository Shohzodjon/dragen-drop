<template>
  <section>
    <transition-group name="flip-list" tag="div">
      <div
        class="w-full flex items-center justify-between p-0.5 border border-white bg-white rounded my-2"
        :class="item.border_color"
        @dragover="(e) => onDragOver(item, i, e)"
        @dragend="(e) => finishDrag(item, i, e)"
        @dragstart="(e) => startDrag(item, i, e)"
        v-for="(item, i) in newArray"
        draggable="true"
        :key="i"
      >
        <input
          type="text"
          class="outline-none p-0.5 px-2"
          v-model="item.data_title"
        />

        <div class="flex gap-2 items-center">
          <div class="relative">
            <button
              :class="item.card_priority"
              id="circle-btn"
              @click="$emit('showColor', item.id)"
            ></button>
            <ul :class="[item.isOpen ? 'cart-drop cart-open' : 'cart-drop']">
              <li
                :class="cartColor.activeClass"
                v-for="(cartColor, index) in cartColors"
                :key="index"
              >
                <button
                  class="p-0.5 h-full w-[100px] text-sm text-white transition duration-300 hover:bg-[#5cacf3]"
                  @click="
                    item.card_priority = cartColor.activeClass;
                    item.border_color = cartColor.border;
                    item.isOpen = false;
                  "
                >
                  {{ cartColor.btnContext }}
                </button>
              </li>
            </ul>
          </div>

          <button
            class="text-lg px-1 transition duration-300 ease-linear hover:text-pink-400"
            @click="$emit('filterInput', item.id)"
          >
            <i class="bi bi-x"></i>
          </button>
        </div>
      </div>
    </transition-group>
  </section>
</template>
<script>
export default {
  props: {
    newArray: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      cartColors: [
        {
          activeClass: "expedite",
          btnContext: "expedite",
          border: "danger_border",
        },
        {
          activeClass: "normal",
          btnContext: "normal priority",
          border: "normal_border",
        },
        {
          activeClass: "critical",
          btnContext: "not critical",
          border: "critical_border",
        },
        {
          activeClass: "priority",
          btnContext: "low priority",
          border: "priority_border",
        },
      ],
      over: {},
      startLoc: 0,
      dragging: false,
      dragFrom: {},
    };
  },
  methods: {
    startDrag(item, i, e) {
      this.startLoc = e.clientY;
      this.dragging = true;
      this.dragFrom = item;
    },
    finishDrag(item, pos) {
      this.newArray.splice(pos, 1);
      this.newArray.splice(this.over.pos, 0, item);
      this.over = {};
    },

    onDragOver(item, pos, e) {
      const dir = this.startLoc < e.clientY ? "down" : "up";
      setTimeout(() => {
        this.over = { item, pos, dir };
      }, 50);
    },
  },
};
</script>
<style>
.expedite {
  background: #dc2626;
}
.normal {
  background: #ffa310;
}
.critical {
  background: #03bb3f;
}
.priority {
  background: #5cacf3;
}
.danger_border {
  border-color: #dc2626 !important;
}
.normal_border {
  border-color: #ffa310 !important;
}
.critical_border {
  border-color: #03bb3f !important;
}
.priority_border {
  border-color: #5cacf3 !important;
}
</style>
