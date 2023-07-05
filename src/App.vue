<template>
    <div class="p-10">
        <h2 class="text-3xl font-thin mb-4">
            Activities
        </h2>

        <table class="mb-8 border-separate -m-4 border-spacing-4">
            <thead>
                <tr>
                    <th class="text-left">
                        Name
                    </th>
                    <th class="text-left">
                        Weight
                    </th>
                    <th class="text-left">
                        Percentage
                    </th>
                    <th>
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr
                    v-for="activity in activities"
                    :key="activity.id"
                >
                    <td>
                        {{ activity.name }}
                    </td>
                    <td>
                        <input
                            type="number"
                            class="border border-gray-400 px-2 w-16"
                            v-model.number="activity.weight"
                        />
                    </td>
                    <td>
                        <input
                            type="number"
                            class="border border-gray-400 px-2 w-16"
                            :value="percentage(activity)"
                            @change="inputPercentage(activity, $event)"
                        />
                        %
                    </td>
                    <td>
                        <button @click="deleteActivity(activity.id)">
                            Delete
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>

        <button
            class="border border-black p-2"
            @click="addActivity"
        >
            Add activity
        </button>
    </div>
</template>

<script setup>
import { ref } from "vue"
import { faker } from "@faker-js/faker"

const activities = ref([
    makeActivity(),
    makeActivity(),
    makeActivity(),
    makeActivity(),
])

function percentage(activity) {
    return (100 * activity.weight / activities.value.reduce((acc, cur) => acc + cur.weight, 0)).toFixed(1)
}

function inputPercentage(activity, event) {
    const { value } = event.target
    let number = parseInt(value, 10)
    if (Number.isNaN(number)) return
    number /= 100
    const numerator = activities.value
        .filter(a => a.id !== activity.id)
        .reduce((acc, cur) => acc + number * cur.weight, 0)
    activity.weight = numerator / (1-number)
}

function makeActivity() {
    return {
        id: faker.string.uuid(),
        name: faker.commerce.productName(),
        weight: 1,
    }
}

function deleteActivity(id) {
    activities.value = activities.value.filter(a => a.id !== id)
}

function addActivity() {
    activities.value.push(makeActivity())
}
</script>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>
