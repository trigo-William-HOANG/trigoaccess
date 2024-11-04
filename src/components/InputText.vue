<template>
  <div class="wave-group">
    <input required type="text" class="input" :name="inputname" />
    <span class="bar"></span>
    <label class="label">
      <span 
        v-for="(char, index) in inputnameArray" 
        :key="index" 
        class="label-char" 
        :style="{ '--index': index }"
      >
        {{ char }}
      </span>
    </label>
    <br>
  </div>
</template>

<script setup>
import { defineProps, computed } from 'vue'

// Define props
const props = defineProps({
  inputname: { type: String, required: true }
})

// Split inputname into an array of characters for dynamic rendering
const inputnameArray = computed(() => props.inputname.split(''))
</script>

<style scoped>
.wave-group {
  position: relative;
  padding-top: 20px;
}

.wave-group .input {
  font-size: 16px;
  padding: 10px 10px 10px 5px;
  display: block;
  width: 200px;
  border: none;
  border-bottom: 1px solid #515151;
  background: transparent;
}

.wave-group .input:focus {
  outline: none;
}

.wave-group .label {
  color: #999;
  font-size: 18px;
  font-weight: normal;
  position: absolute;
  pointer-events: none;
  left: 5px;
  top: 10px;
  display: flex;
}

.wave-group .label-char {
  transition: 0.2s ease all;
  transition-delay: calc(var(--index) * 0.05s);
}

.wave-group .input:focus ~ .label .label-char,
.wave-group .input:valid ~ .label .label-char {
  transform: translateY(-20px);
  font-size: 14px;
  color: #5264AE;
}

.wave-group .bar {
  position: relative;
  display: block;
  width: 200px;
}

.wave-group .bar:before, .wave-group .bar:after {
  content: '';
  height: 2px;
  width: 0;
  bottom: 1px;
  position: absolute;
  background: #5264AE;
  transition: 0.2s ease all;
}

.wave-group .bar:before {
  left: 50%;
}

.wave-group .bar:after {
  right: 50%;
}

.wave-group .input:focus ~ .bar:before,
.wave-group .input:focus ~ .bar:after {
  width: 50%;
}
</style>
