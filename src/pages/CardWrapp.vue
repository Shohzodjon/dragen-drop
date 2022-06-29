<template>
  <section
    class="bg-[#ebecf0] min-w-min p-2 rounded-lg"
    v-for="item in tasks"
    :key="item.id"
  >
    <header class="flex items-center justify-between">
      <div class="flex items-center basis-1/2 gap-2">
        <input
          type="text"
          v-model="title"
          id="title_input"
          :class="color"
          ref="card_title"
        />
        <span>0</span>
      </div>
      <div class="flex items-center gap-3">
        <div class="relative">
          <button
            class="text-lg px-1 rounded-md transition duration-300 ease-linear hover:bg-white"
          >
            <i class="bi bi-three-dots"></i>
          </button>
          <!-- color dropdown list -->
          <ul
            class="absolute left-0 top-7 bg-[#ebecf0] rounded-sm overflow-hidden p-4"
            @click="example"
          >
            <li
              class="transition duration-300 easy-linear py-0.5 px-1 text-sm hover:bg-white"
              v-for="(item, index) in listData"
              :key="index"
            >
              <button :name="item.name">{{ item.contex }}</button>
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

    <TaskInput
      v-for="item in data"
      :key="item.id"
      :item="item"
      @delete="deleteData(id)"
    />

    <div
      class="flex items-center justify-between my-4 mt-5 shadow-md border border-gray-300 rounded-sm py-2"
    >
      <input
        type="text"
        placeholder="Add new task"
        class="bg-[#ebecf0] placeholder:text-black text-sm px-2 outline-none"
        v-model="task_title"
      />
      <button class="text-sm px-1" @click="addData">Create</button>
    </div>
  </section>

  <CreateCart @onclick="addTask" />
</template>
<script>
import TaskInput from "@/components/TaskInput.vue";
import CreateCart from "@/components/CreateCart.vue";

export default {
  components: {
    TaskInput,
    CreateCart,
  },
  data() {
    return {
      color: "",
      title: "Enter list title",
      task_title: "",
      task_result: "untiled",
      tasks: [],
      data: [
        {
          data_title: "untitled",
          id: 1,
        },
      ],

      listData: [
        { contex: "black", name: "color_black" },
        { contex: "red", name: "color_red" },
        { contex: "yellow", name: "color_yellow" },
        { contex: "green", name: "color_green" },
        { contex: "blue", name: "color_blue" },
        { contex: "purple", name: "color_purple" },
        { contex: "pink", name: "color_pink" },
      ],
    };
  },
  methods: {
    example(e) {
      const el = e.target.name;
      if (e.target.name === undefined) return;
      this.color = el;
    },
    //  bu funksiya yange card yaratish uchun ishlatiladi
    addTask() {
      this.tasks.push({
        title: this.title,
        id: Math.random() * 1000,
        task_info: this.data,
      });
      // console.log("tasks", this.tasks);
    },
    //   ########### the end ###########

    //  bu funsiya cardlarni ochirish uchun ishlatiladi
    filterData(id) {
      const filterResult = this.tasks.filter((item) => item.id !== id);
      this.tasks = filterResult;
    },

    // ############ the end ##################

    //   card ichidagi inputga malumot  kiritish uchun  ishlatiladi
    addData() {
      if (this.task_title !== "") {
        this.tasks[0].task_info.push({
          data_title: this.task_result,
          id: Math.random() * 1000,
        });
        this.task_title = "";
        // console.log("result", this.task_title);
      } else {
        alert("Wrong");
      }
    },

    //  ################  the end ######################

    //   bu funksiya  cart ichiga kiritilgan malumotlarni ochirish  uchun ishlatiladi

    deleteData(id) {
      const filteredData = this.tasks[0].task_info.filter(
        (item) => item.id !== id
      );
      alert("wrong");
      this.tasks[0].task_info = filteredData;
    },

    //  ############## the end ################
  },
};
</script>
<style>
#title_input {
  outline: none;
  background: #ebecf0;
}
.drop-list {
  cursor: pointer;
  transition: 0.3s linear all;
  padding: 4px 0;
  font-size: 14px;
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
/*  */
</style>
