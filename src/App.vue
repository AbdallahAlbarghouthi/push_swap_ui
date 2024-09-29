<script setup lang="ts">
import { ref } from 'vue';

const initA = [ 2,1,3,6,5,8, 11, 12, 14, 16, 15, 66, 77, 33, 56, 23];

const stackA = ref([...initA]);
const stackB = ref([]);

const reset = ()=>{
  stackA.value = [...initA];
  stackB.value = [];
  recordedMoves.value = [];
}

const recordedMoves = ref<string[]>([])

const rotate = (stack: number[], name: string) => {
  if (stack.length < 2) {return}
  stack.push(stack.shift()!)

  recordedMoves.value.push(name)
}
const reverseRotate = (stack: number[], name: string) => {
  if (stack.length < 2) {return}
  stack.unshift(stack.pop()!)

  recordedMoves.value.push(name)
}

const push = (from: number[], to: number[], name: string) => {
  if (from.length < 1) {return}
  to.unshift(from.shift()!)

  recordedMoves.value.push(name)
}

const topSwap = (stack: number[], name: string) => {
  if (stack.length < 2) {return}
  const [first, second] = stack.splice(0, 2);
  stack.unshift(first);
  stack.unshift(second);

  recordedMoves.value.push(name)
}


const aMoves = [
  { name: '[sa] Swap A', action: () => topSwap(stackA.value, 'sa')},
  { name: '[pb] Push to B', action: () => push(stackA.value, stackB.value, 'pb')},
  { name: '[ra] Rotate A', action: () => rotate(stackA.value, 'ra')},
  { name: '[rra] Reverse Rotate A', action: () => reverseRotate(stackA.value, 'rra')},
]

const bMoves = [
  { name: '[sb] Swap B', action: () => topSwap(stackB.value, 'sb')},
  { name: '[pa] Push to A', action: () => push(stackB.value, stackA.value, 'pa')},
  { name: '[rb] Rotate B', action: () => rotate(stackB.value, 'rb')},
  { name: '[rrb] Reverse Rotate B', action: () => reverseRotate(stackB.value, 'rrb')},
]
</script>

<template>
  <div class="m-10 h-full">
    <div class="grid grid-cols-2 gap-4 ">
      <div class="bg-green-100 p-4 grid grid-cols-2 rounded-lg">
        <div>
          <button 
            v-for="move in aMoves" 
            class="bg-gray-100 border px-2 py-1 rounded block my-2"
            v-text="move.name"
            @click="move.action()"
          />
        </div>
        <div>
          <div class="text-right">Stack A</div>
          <ul>
            <li v-for="item in stackA" class="text-right font-mono">
              {{ item }}
            </li>
          </ul>
        </div>
      </div>

      <div class="bg-red-100 p-4 grid grid-cols-2 rounded-lg">
        <div>
          <button 
            v-for="move in bMoves" 
            class="bg-gray-100 border px-2 py-1 rounded block my-2"
            v-text="move.name"
            @click="move.action()"
          />
        </div>
        <div>
          <div class="text-right">Stack B</div>
          <ul>
            <li v-for="item in stackB" class=" font-mono text-right">
              {{ item }}
            </li>
          </ul>
        </div>
      </div>
    </div>

    <button @click="reset" class="mt-4 border px-2 py-1 bg-red-900 text-red-100 rounded-xl">Reset</button> 

    <div class="m-4">
      <div v-for="move in recordedMoves">{{ move }}</div>
    </div>
  </div>



</template>

