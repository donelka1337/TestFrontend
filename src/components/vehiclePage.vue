<template>
    <div class = "vehicle">
        <h3>{{vehicleList[index].name}}</h3>
        <p>Модель - {{vehicleList[index].model}}</p>
        <p>Производитель - {{vehicleList[index].manufacturer}}</p>
        <p>Экипаж - {{vehicleList[index].crew}}</p>
        <p>Кол - во пассажиров - {{vehicleList[index].passengers}}</p>
        <p>Грузоподъемность - {{vehicleList[index].cargo_capacity}}</p>
    </div>
  
</template>

<script>
export default {
    props: {
        vehicleList: {
            type: Array,
            required: true,
        },
        index: {
            type: Number,
            default: 0
        }
    }
}
</script>

<style>

</style>