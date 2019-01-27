<template>
  <div class="dropdown" :class="{ 'dropdown--is-open': isOpen }" @click.stop>
    <div class="dropdown__label" @click="toggle">
      <slot name="label"></slot>
    </div>
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
      get: function() {
        return this.options
          ? this.checkedOptions.length === this.options.length
          : false;
      },
      set: function(value) {
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
    toggle: function() {
      this.isOpen = !this.isOpen;
    }
  },
  created: function() {
    const vm = this;
    document.addEventListener("click", function() {
      vm.isOpen = false;
    });
  },
  mounted: function() {
    this.selectAll = true;
  }
};
</script>

<style scoped>
.dropdown {
  position: relative;
  display: block;
  margin: 1em;
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
  border-radius: 8px;
  min-width: 100%;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.201285);
}

.dropdown__option {
  display: block;
  width: 170px;
  height: 35px;
  line-height: 35px;
  font-size: 14px;
  position: relative;
}

.dropdown__option:hover {
  background-color: #f2f2f2;
}

.dropdown__checkbox {
  -webkit-appearance: none;
  background-color: #fafafa;
  border: 1px solid #fe9922;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05),
    inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05);
  padding: 9px;
  border-radius: 5px;
  display: inline-block;
  position: relative;
}

.dropdown__checkbox:checked {
  background-color: #fe9922;
  color: #ffffff;
}

.dropdown__checkbox:checked:after {
  content: "";
  display: block;
  position: absolute;
  top: 5px;
  left: 2px;
  background: url("../assets/images/icons/check.svg") no-repeat;
  background-size: 15px 10px;
  width: 15px;
  height: 10px;
}

.dropdown__checkbox:focus {
  outline: 0;
}
</style>
