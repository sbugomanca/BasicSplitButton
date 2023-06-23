<template>
    <div class = "bsb">
        <div class="bsb-container">
            <div :class="'bsb-btn p-3 ' + (openDD ? 'rounded-tl-xl' :'rounded-l-xl') + ' ' + btnType" @click="onClickDefaultOption">
                {{ defaultOptionLabel }}
            </div>
            <div :class="'bsb-btn p-3 ' + (openDD ? 'rounded-tr-xl' : 'rounded-r-xl') + ' '  + btnType" @click="onClickDropdownIcon()">
                <svg id="dd-icon" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 8">
                    <template v-if="!openDD">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 5.326 5.7a.909.909 0 0 0 1.348 0L13 1"/>
                    </template>
                    <template v-else>
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7 7.674 1.3a.91.91 0 0 0-1.348 0L1 7"/>
                    </template>

                </svg>
                
            </div>            
        </div>
        <div :hidden="!openDD" :class="'bsb-dropdown-container rounded-br-xl rounded-bl-xl p-0 ' + btnType">
            <div v-for="option in optionsList" :key="option" class="rounded-b-xl">
                <div @click="onSelect" class="bsb-dropdown-container-list-el rounded-b-xl">{{ option }}</div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import {defineComponent} from 'vue'


export default defineComponent({
    props:{
        options: {
            type: Object ,
            required: true
        },
        defaultValue : {
            type: Number,
            default: 0
        },
        btnType:{
            type: String,
            default: "standard"
        }
    },
    emits: ['select'],
    mounted() {
        let optionsString = typeof this.options == "string" ? this.options : this.options.toString()

        this.optionsList = optionsString?.split(',', 1000)
        this.defaultOptionLabel = this.optionsList.shift()

    },
    data() {
        return {
            defaultOptionLabel: "", 
            optionsList: {},
            openDD: false,
            selected: "",

            onClickDefaultOption : () => {
                this.selected = this.defaultOptionLabel                
                this.$emit("select", this.selected)
            },
            onClickDropdownIcon: () => this.openDD = !this.openDD,
            onSelect: (event:any) => {
                this.selected = event.target.textContent
                console.table("Selected: ",event.target.textContent)
                this.onClickDropdownIcon()
                
                this.$emit("select", this.selected)
            }
        }
    }
   
})
</script>

<style scoped>
    .bsb-container{
        display: flex;
        flex-direction: row;
        justify-content: center;
    }
    .bsb-container:hover{
       border-color: black;
    }
    .bsb-container .bsb-btn, .bsb-dropdown-container{
        background: radial-gradient(#fffffa, #0000003b);
        color: black;
        width: 100%;
        border: 1px solid black;     
    }
   
    .bsb-container .bsb-btn.warning , .bsb-dropdown-container.warning {
        background: radial-gradient(#ffe500, transparent);
    }
    .bsb-container .bsb-btn.error , .bsb-dropdown-container.error {
        background: radial-gradient(#e21e1e, #0000003b);
        color:white;      
        border: 1px solid white;   
    }

    .bsb-container .bsb-btn:hover, .bsb-container .bsb-btn:focus, .bsb-dropdown-container .bsb-dropdown-container-list-el:hover, .bsb-dropdown-container-list-el:hover.focus{      
        opacity: 0.8;
        cursor: pointer;
    }

    .bsb-dropdown-container{
        width: 100%;
    }

    #dd-icon{
        width: 15px;
        height: 25px;
    }
</style>