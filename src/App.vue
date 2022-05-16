<template>
    <modal v-model:show="modalVisible" @updateShow ="updateShow">
     <div v-if="this.vehicles.length > 0">
        <vehicle-item
        :vehicleList = "vehicles"
        :index = "index"
        />
        <div class = "buttons">
            <button v-if = "this.index != 0" class = "leftButton" @click.stop="decreaseIndex">Предыдущая страница</button>
            <button v-if = "this.index != this.vehicles.length - 1" class = "rightButton" @click.stop="increaseIndex">Следующая страница</button>
        </div>
    </div>
    <div v-else>
        <spinner class ="modalSpinner"/>
    </div>
    </modal>
    <div class = "container">
        <div class = "left">
        </div>
        <div class = "center">
            <div v-if="characters.length > 0">
        <div class = "head"> <h1>Персонажи Star Wars</h1> </div>
        <div class = "buttons">
         <button v-if="this.prevPage != null && this.prevPage.length != 0" class = "leftButton" @click="goToPrev">Предыдущая страница</button>
         <button v-if="this.nextPage != null && this.nextPage.length != 0" class = "rightButton" @click="goToNext">Следующая страница</button>
         </div>
        <character-list
        :characters="characters"
        @getCharacter = "createInfo"
        />
        </div>
            <div v-else>
                <spinner class ="bodySpinner"/>
            </div>
        </div>
         <div class = "right">
            
        </div>
        
    </div>
</template>

<script>
import spinner from "@/components/ui/spinner.vue"
import characterList from "@/components/characterList.vue"
import modal from "@/components/ui/modal.vue"
import vehicleItem from "@/components/vehiclePage.vue"
import axios from "axios"
export default {
    components: {characterList, spinner, modal, vehicleItem},
    methods:{

        increaseIndex(){
            if(this.index < this.vehicles.length - 1) {
            this.index += 1;
            }
        },

        decreaseIndex(){
            if(this.index > 0) {
            this.index -= 1;
            }
        },

        updateShow(modalVisibleStatus) {
            this.modalVisible = modalVisibleStatus;
            this.vehicles = [];
            this.index = 0;
        },

        createInfo(characterInfo) {
            if(characterInfo.vehiclePages.length != 0){
                this.modalVisible = true;
                Promise.all(characterInfo.vehiclePages.map(el => axios.get(el))).then((resolve)=>{
                    const result = resolve.map(el => el.data)
                    this.vehicles = result
                })
            }
        },
        async goToNext(){
            if(this.nextPage != null && this.nextPage.length != 0) {
                this.characters = [];
                this.current = this.nextPage;
                await this.getData();
            }
        },
        async goToPrev(){
            if(this.prevPage != null && this.prevPage.length != 0) {
                this.characters = [];
                this.current = this.prevPage;
                await this.getData();
            }
        },
        async getData(){
               await axios.get(this.current).then((resolve) =>{
                const result = []
                const character = {}
                resolve.data.results.forEach(el => {
                    character.name = el.name;
                    character.height = el.height;
                    character.mass = el.mass;
                    character.hair_color = el.hair_color;
                    character.skin_color = el.skin_color;
                    character.eye_color = el.eye_color;
                    character.birth_year = el.birth_year;
                    character.gender = el.gender
                    character.vehiclePages = el.vehicles
                    result.push(JSON.parse(JSON.stringify(character)));
                });
                this.prevPage = resolve.data.previous;
                this.nextPage = resolve.data.next;
                this.characters = result;
            });
         
        }
    },
    mounted(){
        this.current = 'https://swapi.dev/api/people';
        this.getData();
        
    },
   data() {
       return  { characters: [],
            prevPage: '',
            nextPage: '',
            current: '',
            vehicles: [],
            modalVisible: false,
            index: 0
           }
   }
}
</script>

<style>
    *,*::before, *::after{
        margin: 0;
        padding: 0;
        border: 0;
    }
    .buttons{
        overflow: hidden;
    }
    .leftButton {
        float:left;
    }
    .rightButton {
        float:right;
    }
    .center {
        background-color: black;
    }
    .left {
        background-image: url("@/static/SW-background.jpg");
    }
    .right {
        background-image: url("@/static/SW-background.jpg");
    }
    .modalVehicle{
        background-color: black;
    }
    .bodySpinner{
        height: 100vh;
    }
    .modalSpinner{
        height: 50vh;
    }
    .container{
        display: grid;
        grid-template-columns: 1fr 2fr 1fr;
        min-height: 100vh;
        
    }
    p,h3,h1{
        color:white;
    }
    .head{
        text-align: center;
    }
</style>