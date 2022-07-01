<template>
  <transition-group
    name="flip-list"
    class="flex mt-4 gap-7 flex-wrap items-start"
    tag="div"
  >
    <section
      class="bg-[#ebecf0] min-w-min p-2 rounded-lg"
      v-for="(item, i) in tasks"
      @dragover="(e) => onDragOver(item, i, e)"
      @dragend="(e) => finishDrag(item, i, e)"
      @dragstart="(e) => startDrag(item, i, e)"
      :key="item.id"
      draggable="true"
    >
      <header class="flex items-center justify-between">
        <div class="flex items-center basis-1/2 gap-2">
          <input
            type="text"
            v-model="item.title"
            id="title_input"
            :class="item.title_color"
          />
          <span>{{ item.cardData.length }}</span>
        </div>
        <div class="flex items-center gap-3">
          <div class="relative">
            <button
              class="text-lg px-1 rounded-md transition duration-300 ease-linear hover:bg-white"
              @click="showDrop(item.id)"
            >
              <i class="bi bi-three-dots"></i>
            </button>
            <!-- color dropdown list -->
            <ul :class="[item.show ? 'drop-down drop-active' : 'drop-down']">
              <li
                class="transition duration-300 easy-linear py-0.5 px-1 text-sm hover:bg-white"
                v-for="(data, index) in listData"
                :key="index"
              >
                <button
                  @click="
                    item.title_color = data.name;
                    item.show = false;
                  "
                >
                  {{ data.contex }}
                </button>
              </li>
            </ul>
            <!--  end of color dropdown  -->
          </div>

          <button
            class="text-lg px-1 rounded-md transition duration-300 ease-linear hover:bg-white"
            @click="filterData(item.id)"
          >
            <i class="bi bi-x"></i>
          </button>
        </div>
      </header>

      <!-- card input -->
      <!-- drag and drop -->

      <div
        class="w-full flex items-center justify-between p-0.5 bg-white rounded my-2"
        v-for="(info, i) in item.cardData"
        draggable="true"
        :key="info"
      >
        <input
          type="text"
          class="outline-none p-0.5 px-2"
          v-model="info.data_title"
        />

        <div class="flex gap-2 items-center">
          <div class="relative">
            <button
              :class="info.card_priority"
              id="circle-btn"
              @click="showCartColor(info.id)"
            ></button>
            <ul :class="[info.isOpen ? 'cart-drop cart-open' : 'cart-drop']">
              <li
                :class="cartColor.activeClass"
                v-for="(cartColor, i) in cartColors"
              >
                <button
                  class="p-0.5 h-full w-[100px] text-sm text-white transition duration-300 hover:bg-[#5cacf3]"
                  @click="
                    info.card_priority = cartColor.activeClass;
                    info.isOpen = false;
                  "
                >
                  {{ cartColor.btnContext }}
                </button>
              </li>
            </ul>
          </div>

          <button
            class="text-lg px-1 transition duration-300 ease-linear hover:text-pink-400"
            @click="filterCardData(info.id)"
          >
            <i class="bi bi-x"></i>
          </button>
        </div>
      </div>

      <!--  end of drag and drop -->

      <form
        @submit.prevent="pushData(item.id)"
        class="flex items-center justify-between my-4 mt-5 shadow-md border border-gray-300 rounded-sm py-2"
      >
        <input
          type="text"
          placeholder="Add new task"
          class="bg-[#ebecf0] placeholder:text-black text-sm px-2 outline-none"
          v-model="item.inputText"
        />

        <button class="text-sm px-1">Create</button>
      </form>
    </section>
  </transition-group>

  <CreateCart @onclick="addCard" />
</template>
<script>
import CreateCart from "@/components/CreateCart.vue";

export default {
  components: {
    CreateCart,
  },
  data() {
    return {
      tasks: [],
      startLoc: 0,
      dragging: false,
      dragFrom: {},
      listData: [
        { contex: "black", name: "color_black" },
        { contex: "red", name: "color_red" },
        { contex: "yellow", name: "color_yellow" },
        { contex: "green", name: "color_green" },
        { contex: "blue", name: "color_blue" },
        { contex: "purple", name: "color_purple" },
        { contex: "pink", name: "color_pink" },
      ],
      cartColors: [
        { activeClass: "expedite", btnContext: "expedite" },
        { activeClass: "normal", btnContext: "normal priority" },
        { activeClass: "critical", btnContext: "not critical" },
        { activeClass: "priority", btnContext: "low priority" },
      ],
    };
  },
  watch: {
    tasks: {
      handler(newValue) {
        const data = JSON.stringify(newValue);
        window.localStorage.setItem("task", data);
      },
      deep: true,
    },
  },
  methods: {
    // drag and drop functions
    startDrag(item, i, e) {
      this.startLoc = e.clientY;
      this.dragging = true;
      this.dragFrom = item;
      console.log(this.dragFrom);
    },
    finishDrag(item, pos) {
      this.tasks.splice(pos, 1);
      this.tasks.splice(this.over.pos, 0, item);
      this.over = {};
    },

    onDragOver(item, pos, e) {
      const dir = this.startLoc < e.clientY ? "down" : "up";
      setTimeout(() => {
        this.over = { item, pos, dir };
      }, 50);
    },
    //  ############  the end of drag and drop function ###########
    //  bu funksiya yangi card yaratish uchun ishlatiladi
    addCard() {
      this.tasks.push({
        title: "Enter list title",
        id: Math.random() * 10000,
        show: false,
        title_color: "",
        inputText: "",
        cardData: [
          {
            data_title: "untitled",
            id: Math.random() * 10000,
            isOpen: false,
            card_priority: "expedite",
          },
        ],
      });
    },
    //   ########### the end ###########

    //  bu funsiya cardlarni ochirish uchun ishlatiladi
    filterData(id) {
      const filterResult = this.tasks.filter((item) => item.id !== id);
      this.tasks = filterResult;
    },

    // ############ the end ##################

    //  show dropdown
    showDrop(id) {
      this.tasks.forEach((item) => {
        if (item.id === id) {
          item.show = !item.show;
        }
      });
    },
    // ################## the end ##########################

    //
    pushData(id) {
      this.tasks.forEach((item) => {
        if (item.id === id && item.inputText !== "") {
          item.cardData.push({
            data_title: item.inputText,
            id: Math.random() * 10000,
            isOpen: false,
            card_priority: "expedite",
          });
          item.inputText = "";
          id = 0;
        }
      });
    },

    //  card ichidagi malumotlarini filterlash
    filterCardData(id) {
      this.tasks.forEach((item) => {
        let newCard = item.cardData.filter((i) => i.id !== id);
        item.cardData = newCard;
      });
    },

    showCartColor(id) {
      this.tasks.forEach((item) => {
        item.cardData.forEach((i) => {
          if (i.id === id) {
            i.isOpen = !i.isOpen;
          }
        });
      });
      console.log("task", this.tasks);
    },
    getLocalStorage() {
      const data = localStorage.getItem("task");
      this.tasks = JSON.parse(data);
    },
  },
  mounted() {
    this.getLocalStorage();
  },
};
</script>
<style>
#title_input {
  outline: none;
  background: #ebecf0;
  width: 80%;
  margin-right: 5px;
}
/*  drop-down list class */
.drop-down {
  position: absolute;
  left: 0;
  top: 28px;
  background: #ebecf0;
  border-radius: 6px;
  overflow: hidden;
  transition: 0.3s all linear;
  height: 0;
  z-index: 22;
}
.drop-active {
  height: 120px;
}

.drop-list {
  cursor: pointer;
  transition: 0.3s linear all;
  padding: 4px 0;
  font-size: 14px;
  margin: 4px;
  padding: 4px;
}
.drop-list:hover {
  background: #9ca3af;
}
/* color classes */
.color_black {
  color: #000;
}
.color_red {
  color: #ef4444;
}
.color_yellow {
  color: #f59e0b;
}
.color_green {
  color: #10b981;
}
.color_blue {
  color: #3b82f6;
}
.color_purple {
  color: #8b5cf6;
}
.color_pink {
  color: #ec4899;
}
/*cart  circle color  */
.cart-drop {
  position: absolute;
  left: 0;
  top: 23px;
  border-radius: 4px;
  height: 0;
  overflow: hidden;
  transition: 0.3s linear all;
  z-index: 222;
}
.cart-open {
  height: 120px;
}
#circle-btn {
  width: 15px;
  height: 15px;
  border-radius: 100%;
}
/* colors */
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
</style>
<!--  :class="{
          over: info === over.info && info !== dragFrom,
          [over.dir]: item === over.info && info !== dragFrom,
        }" -->
