<template>
  <div class="h-screen flex flex-col bg-gray-600 text-gray-900">
    <Header />

    <div class="flex flex-grow justify-center items-center">
      <div class="flex flex-col flex-grow justify-center items-center">
        <ul class="w-3/4 pb-2 bg-gray-500 rounded-xl">
          <li class="py-2 mb-2 bg-white rounded-xl text-center font-bold">
              Wejście
          </li>
          <li 
            v-for="(el, i) in registers" 
            :key="i + '-input'"
            class="h-14 flex gap-10 justify-center items-center"
          >
            <label class="w-12 h-10 py-2 bg-white rounded-xl text-center font-bold uppercase" :for="i">{{ i }}</label>

            <div class="h-full border-r"></div>

            <input :data-input="i" type="text" maxlength="6" class="h-10 px-2 bg-white rounded-xl">
          </li>
        </ul>

        <div class="w-3/4 mt-3 pb-3 flex flex-col bg-gray-500 rounded-xl">
          <span class="py-2 mb-4 bg-white rounded-xl text-center font-bold">
            Operacje
          </span>

          <div class="mx-auto flex flex-shrink flex-grow-0 gap-10">
            <button class="py-2 px-5 bg-white rounded-xl font-bold" @click="saveRegisters">Save</button>
            <button class="py-2 px-5 bg-white rounded-xl font-bold" @click="resetRegisters">Reset</button>
            <button class="py-2 px-5 bg-white rounded-xl font-bold" @click="randomRegisters">Random</button>
          </div>
        </div>
      </div>

      <div class="flex flex-col items-center flex-grow">
        <div class="w-full">
          <div class="w-full flex flex-grow gap-3">
            <div class="pb-3 flex flex-grow bg-gray-500 rounded-xl">
              <ul class="flex flex-col flex-grow gap-3">
                <li class="flex gap-3 justify-center">
                
                  <span class="w-full h-10 py-2 bg-white rounded-xl text-center font-bold">
                    Z
                  </span>
                </li>

                <li 
                  v-for="(el, i) in registers" 
                  :key="i + '-radioFrom'" 
                  class="flex gap-3 justify-center"
                >
                  <label class="w-12 h-10 py-2 bg-white rounded-xl text-center font-bold uppercase" for="from">
                    {{ i }}
                  </label>

                  <input 
                    v-model="radio.from" 
                    type="radio"
                    name="from"
                    :value="i"
                  >
                </li>
              </ul>
            </div>

            <div class="pb-3 flex flex-grow bg-gray-500 rounded-xl">
              <div class="flex flex-grow">
                <ul class="flex flex-col flex-grow gap-3">
                  <li class="flex gap-3 justify-center">
                
                  <span class="w-full h-10 py-2 bg-white rounded-xl text-center font-bold">
                    Do
                  </span>
                </li>

                  <li
                    v-for="(el, i) in registers"
                    :key="i + '-radioTo'"
                    class="flex gap-3 justify-center"
                  >
                    <label class="w-12 h-10 py-2 bg-white rounded-xl text-center font-bold uppercase" for="to">
                      {{ i }}
                    </label>

                    <input
                      v-model="radio.to"
                      type="radio" 
                      name="to" 
                      :value="i"
                    >
                  </li>
                </ul>
              </div>
            </div>
          </div>

          <div class="pb-3 mt-3 flex flex-col gap-4 bg-gray-500 rounded-xl">
            <span class="w-full h-10 py-2 bg-white rounded-xl text-center font-bold">
              Operacje
            </span>

            <div class="flex gap-10 justify-center">
              <button class="py-2 px-5 bg-white rounded-xl font-bold" @click="move">MOVE</button>
              <button class="py-2 px-5 bg-white rounded-xl font-bold" @click="exch">EXCH</button>
            </div>
          </div>
        </div>
      </div>


      <div class="flex flex-grow justify-center">
        <ul class="w-3/4 pb-2 bg-gray-500 rounded-xl">
          <li class="py-2 mb-2 bg-white rounded-xl text-center font-bold">
            Wyjście
          </li>
          <li
            v-for="(el, i) in registers"
            :key="i + '-list'"
            class="w-full h-14 flex gap-10 justify-center items-center"
          >
            <span class="w-12 h-10 py-2 bg-white rounded-xl text-center font-bold uppercase">
              {{ i }}
            </span>

            <div class="h-full border-r"></div>

            <span class="w-20 h-10 py-2 bg-white rounded-xl text-center font-bold uppercase">
              {{ el }}
            </span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import { set } from "vue";
import Header from "../components/Header.vue";

const resetVal = '000000'

export default {
  name: "IndexPage",
  components: { Header },
  data: () => ({
    registers: {
      ah: resetVal,
      al: resetVal,
      bh: resetVal,
      bl: resetVal,
      ch: resetVal,
      cl: resetVal,
      dh: resetVal,
      dl: resetVal,
    },
    radio: {
      from: 'ah',
      to: 'ah',
    }
  }),
  methods: {
    saveRegisters() {
      const inputs = document.querySelectorAll("[data-input]");

      inputs.forEach(el => {
        const registerKey = el.getAttribute("data-input");
        this.registers[registerKey] = el.value || this.registers[registerKey];
      });
    },
    resetRegisters() {
      const inputs = document.querySelectorAll("[data-input]");

      inputs.forEach(el => {
        const registerKey = el.getAttribute("data-input");

        el.value = '';
        this.registers[registerKey] = resetVal;
      });
    },
    randomRegisters() {
      function randomHex() {
        return Math.floor(Math.random() * 16777215).toString(16);
      }

      for (const el in this.registers) {
        this.registers[el] = randomHex();
      }
    },

    move() {
      set(this.registers, this.radio.to, this.registers[this.radio.from]);
    },
    exch() {
      const temp = this.registers[this.radio.from];

      set(this.registers, this.radio.from, this.registers[this.radio.to]);
      set(this.registers, this.radio.to, temp);
    }
  },
}
</script>
