<template>
<div>
    <character-item
    @getCharacter="a"
    v-for="character in characters"
    :character="character"
    :key="character.name"
    />
</div>
  
</template>

<script>
import characterItem from "@/components/characterItem";
export default {
    methods:{
        a(character){
            this.$emit('getCharacter',character);
        }
    },
    components:{characterItem},
    props:{
        characters: {
            type: Array,
            required: true,
        }
    }
}
</script>

<style>

</style>