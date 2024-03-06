<script setup>
import { ref, reactive, computed } from 'vue';

const myMessage = "Yellow World"

const sortConfig = reactive({
    field: "id",
    direction: "asc"
})

const workflows = reactive({
    value: [
        { id: 1, name: "Workflow 1", status: "Success" },
        { id: 2, name: ". Workflow 2", status: "Failed" },
        { id: 3, name: ". Workflow 2.1", status: "Failed" },
        { id: 4, name: "Workflow 3", status: "Success" },
        { id: 5, name: "Workflow 5", status: "Success" },
        { id: 6, name: "Workflow 6", status: "Success" },
        { id: 7, name: "Workflow 7" }
    ]
})

const filteredWorkflows = computed(() => workflows.value.filter(item => filterArray.value.includes("all") ? item : filterArray.value.includes(item.status?.toLowerCase()) ));
const filteredAndSortedWorkflows = computed(() => filteredWorkflows.value.slice().sort((a, b) => sortConfig.direction === "asc" ? a[sortConfig.field]?.toString().localeCompare(b[sortConfig.field].toString()): b.name.localeCompare(a.name)));



// Ref is desined for reactive single values
const filterArray = ref(["all"])

function setFilter(event) {
    filterArray.value.push = event.target.id
}

function toggleFilter(filter) {
    if (filter === "all") {
        filterArray.value = ["all"]
        return
    }
    (!filterArray.value.includes(filter)) ? filterArray.value.pop("all") && filterArray.value.push(filter) : filterArray.value.pop(filter)
}

function setSortBy(fieldName) {
    if (sortConfig.field === fieldName) {
        sortConfig.direction = sortConfig.direction === "asc" ? "desc" : "asc";
    } else {
        sortConfig.direction = "asc";
    }
    sortConfig.field = fieldName;
}



</script>

<template>
    <div>
        <h1>{{ myMessage }}</h1>
        {{  filterArray.includes("all") }}
        <button id="Success" @click='toggleFilter("success")' :class='filterArray.includes("success") ? "active bold" : ""'>Success</button>
        <button id="Failed" @click='toggleFilter("failed")' :class='filterArray.includes("failed")  ? "active bold" : ""'>Failed</button>
        <button id="All" @click='toggleFilter("all")' :class='filterArray.includes("all") ? "active bold" : ""'>All</button>

        <table>
            <thead>
                <tr>
                    <th @click='setSortBy("id")' >ID</th>
                    <th @click='setSortBy("name")' >Workflow Name</th>
                    <th @click='setSortBy("status")' >Status</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="workflow in filteredAndSortedWorkflows" :key="workflow.id">
                    <td>{{ workflow.id }}</td>
                    <td>{{ workflow.name }}</td>
                    <td>{{ workflow.status || "Unkown" }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<style>
.active {
    background-color: red;
}

.bold {
    font-weight: bold;
}
</style>