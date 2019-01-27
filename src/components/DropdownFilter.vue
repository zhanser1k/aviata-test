<template>
  <div class="dropdown" @click.stop>
    <div class="dropdown__label" @click="toggle">
      <span
        class="dropdown__label__text"
        :class="{ 'dropdown__is-open': isOpen }"
        ><slot name="label"></slot
      ></span>
    </div>
    <transition
      name="dropdown-transition"
      enter-active-class="animated fadeIn faster"
      leave-active-class="animated fadeOut faster"
    >
      <div v-show="isOpen" class="dropdown__options">
        <label class="dropdown__option">
          <input
            class="dropdown__checkbox"
            type="checkbox"
            v-model="selectAll"
            @change="$emit('option-click', checkedOptions)"
          />
          Все
        </label>
        <label
          class="dropdown__option"
          v-for="(option, index) in options"
          :key="`option-${index}`"
        >
          <input
            type="checkbox"
            class="dropdown__checkbox"
            v-model="checkedOptions"
            :value="option.value"
            @change="$emit('option-click', checkedOptions)"
          />
          {{ option.title }}
        </label>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "DropdownFilter",
  data() {
    return {
      checkedOptions: [],
      isOpen: false
    };
  },
  props: {
    options: {
      type: Array,
      required: true
    }
  },
  computed: {
    selectAll: {
      get() {
        return this.options
          ? this.checkedOptions.length === this.options.length
          : false;
      },
      set(value) {
        let checkedOptions = [];

        if (value) {
          this.options.forEach(option => {
            checkedOptions.push(option.value);
          });
        }

        this.checkedOptions = checkedOptions;
      }
    }
  },
  methods: {
    toggle() {
      this.isOpen = !this.isOpen;
    }
  },
  created() {
    const vm = this;
    document.addEventListener("click", function() {
      vm.isOpen = false;
    });
  },
  mounted() {
    this.selectAll = true;
    this.$emit("option-click", this.checkedOptions);
  }
};
</script>

<style scoped>
.dropdown {
  position: relative;
  display: block;
  margin: 1em 0;
}

.dropdown__label,
.dropdown__option {
  z-index: 1;
  position: relative;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.dropdown__options {
  z-index: 10;
  position: absolute;
  top: 100%;
  left: 0;
  padding: 10px 0;
  border-radius: 8px;
  min-width: 100%;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.201285);
}

.dropdown__option {
  display: block;
  width: 170px;
  height: 35px;
  padding-left: 10px;
  line-height: 35px;
  font-size: 14px;
  position: relative;
}

.dropdown__option:hover {
  background-color: #f2f2f2;
}

.dropdown__checkbox {
  display: inline-block;
  vertical-align: middle;
  -webkit-appearance: none;
  background-color: #fafafa;
  border: 1px solid #fe9922;
  padding: 9px;
  border-radius: 5px;
  position: relative;
  margin-right: 10px;
}

.dropdown__checkbox:checked {
  background-color: #fe9922;
  color: #ffffff;
}

.dropdown__checkbox:checked:after {
  content: "";
  position: absolute;
  top: 5px;
  left: 2px;
  background: url("../assets/images/icons/check.svg") no-repeat;
  background-size: 15px 10px;
  width: 15px;
  height: 10px;
}

.dropdown__label__text:after {
  content: "";
  position: absolute;
  background: url("../assets/images/icons/dropdown-arrow-up.svg") no-repeat;
  transform: rotate(180deg);
  transition: 300ms;
  background-size: 11px 6px;
  width: 11px;
  height: 6px;
  top: 7px;
  left: 107px;
}

.dropdown__is-open:after {
  transform: rotate(0);
}

.dropdown__checkbox:focus {
  outline: 0;
}
</style>
