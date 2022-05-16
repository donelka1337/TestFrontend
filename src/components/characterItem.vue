<template>
    <div @click="test" class = "character">
        <h3>{{character.name}}</h3>
        <p>рост - {{character.height}}</p>
        <p>цвет волос - {{character.hair_color}}</p>
        <p>цвет кожи - {{character.skin_color}}</p>
        <p>цвет глаз - {{character.eye_color}}</p>
        <p>пол - {{character.gender}}</p>
        <p v-if="character.vehiclePages.length > 0">Нажмите, чтобы узнать о транспортных средствах этого персонажа</p>
    </div>
  
</template>

<script>
export default {
    methods:{
        test(){
            this.$emit('getCharacter',this.character);
        }
    },
    props: {
        character: {
            type: Object,
            required: true,
        }
    }
}
</script>

<style>
    .character{
        border: 1px solid white;
    }
</style>