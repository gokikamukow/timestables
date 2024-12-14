<script setup>
import { ref, defineProps, defineEmits, onMounted, onUnmounted, watch } from 'vue'

const props = defineProps({
    min: Number,
    max: Number,
    trials: Number
})

const start_time = ref(null)
const elapsed = ref(0)
const correct = ref(0)
const incorrect = ref(0)
const total_time = ref(null)
const already_tested = ref([])

const left = ref(null)
const right = ref(null)

const theinput = ref(null)
const msg = ref('')

const timer = ref(null);

function makeNewProblem() {
    const unconstrainedMax = 12
    const constrained = Math.floor(Math.random() * (props.max - props.min + 1) + props.min)
    const unconstrained = Math.floor(Math.random() * unconstrainedMax + 1)

    if (Math.random() > 0.5) {
        [left.value, right.value] = [constrained, unconstrained]
    } else {
        [left.value, right.value] = [unconstrained, constrained]
    }
}

onMounted(() => {
    makeNewProblem()
    timer.value = setInterval(() => {
        elapsed.value = new Date() - start_time.value
    }, 100)
})

function restart() {
    makeNewProblem()
    start_time.value = null;
    correct.value = 0;
    incorrect.value = 0;
    already_tested.value = [];
}

watch(() => [props.min, props.max, props.trials], () => {
    restart()
})

onUnmounted(() => {    
    clearInterval(timer.value)
})

function oninput(event) {
    if (event.key === 'Enter' && event.target.value != "") {
        if (left.value * right.value === Number(event.target.value)) {
            msg.value = 'Correct!'
            correct.value++

            var at = Array.from(already_tested.value, x => Array.from(x))
            at = at.concat([[left.value, right.value]])

            do {
                makeNewProblem()
            } while (at.includes([left.value, right.value]))

            already_tested.value = at

            if (correct.value == props.trials) {
                clearInterval(timer.value)
                total_time.value = new Date() - start_time.value
                msg.value = 'Finished!'
            }
        } else {
            incorrect.value++
            msg.value = 'Try again!'
        }

        if (start_time.value == null) {
            start_time.value = new Date()
        }

        event.target.value = ""
    }
}



</script>

<template>
    <w-button
        @click="restart">
    Restart
    </w-button>
    <w-card
    title="Times Tables"
    color="indigo-dark6"
    class="sh6"
    style="transform:scale(1.5);transform-origin: top;"
    >
        <w-flex column>
            <w-flex v-if="total_time == null">
                <div>{{ left }} x {{ right }} = </div>
                <w-input
                v-model="theinput"
                @keydown="oninput"
                color="indigo-dark6"
                type="number"
                />
            </w-flex>
            <br>
            <w-flex>
                {{ msg }}
            </w-flex>
            <br>
            <w-flex v-if="start_time != null">
                <div>{{ correct }}/{{ props.trials }}</div>
                <div v-if="total_time != null">, took {{ Math.round(elapsed / 1000) }} s</div>
            </w-flex>
        </w-flex>
    </w-card>

  <div class="card">
    
  </div>

</template>

<style scoped></style>
