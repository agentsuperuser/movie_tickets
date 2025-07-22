<template>
    <div class=" justify-center">
        <h1 class="text-gray-900 font-bold text-[32px]">Predator</h1>
        <div class="mt-11 flex flex-row justify-between">
            <div class="flex flex-col space-y-3">
                <h2 class="text-base font-bold uppercase text-gray-700">Director</h2>
                <p class="text-xl font-semibold text-gray-600">Name</p>
            </div>

            <div class="flex flex-col space-y-3">
                <h2 class="text-base font-bold uppercase text-gray-700">Release Date</h2>
                <p class="text-xl font-semibold text-gray-600">date</p>
            </div>
        </div>


        <div class="max-w-full mx-12">

            <div v-if="currentStep == 0">
                <div class="flex justify-center mx-12 mt-7 bg-gray-300 p-2 rounded shadow-2xl">
                    <img class="rounded" src="http://movies.localhost:8000/files/predator.jpeg" alt="Poster">
                </div>
    
                <div class="w-full flex items-center justify-center mt-5">
                    <Button size="xl" variant="solid" @click="currentStep++">Book Tickets</Button>
                </div>
    
                <div class="flex flex-col space-y-3 mt-16">
                    <h2 class="text-base font-bold uppercase text-gray-700">Synopsis</h2>
                    <p class="text-lg font-normal text-gray-600">A Viking raider, a ninja in feudal Japan, and a World War II pilot encounter a fierce intergalactic hunter.</p>
                </div>
            </div>


            <div v-else-if="currentStep === 1">
                <h2 class="font-medium text-xl mt-7 text-gray-900">How many Seats ?</h2>

                <div class="flex flex-col w-full space-y-5 mt-6">
                    <Button 
                        size="lg" 
                        :variant="index === bookingData.numberOfSeats ? 'subtle' : 'white'" 
                        class="shadow-lg" 
                        @click="setNumberOfSeats(index)"
                        v-for="(index) in 8"
                    >{{ index }}</Button>
                </div>
            </div>

            <div v-else-if="currentStep === 2">
                <div class="flex flex-col space-y-4">
                    <h2 class="font-medium text-xl mt-7 text-gray-900">Date</h2>
                    <Input class="h-10" v-model="bookingData.date" />
                </div>

                <div class="flex flex-col space-y-4">
                    <h2 class="font-medium text-xl mt-7 text-gray-900">Cinema & Shows</h2>

                    <div class="space-y-3">
                        <div class="bg-white shadow-xl p-4 rounded flex flex-col space-y-4">
                            <h3 class="text-sm font-bold text-gray-800">PVR</h3>
                            <div class="flex flex-row space-x-2">
                                <Button size="sm" variant="outline">12:30PM</Button>
                                <Button size="sm" variant="outline">3:30PM</Button>
                            </div>
                        </div>

                        <div class="bg-white shadow-xl p-4 rounded flex flex-col space-y-4">
                            <h3 class="text-sm font-bold text-gray-800">Star Talkies</h3>
                            <div class="flex flex-row space-x-2">
                                <Button size="sm" variant="outline">11:10PM</Button>
                            </div>
                        </div>
                    </div>
                </div>
                
            </div>

            <div v-else-if="currentStep === 3">
                <h2 class="font-medium text-xl mt-7 text-gray-900">Select Seats</h2>

                <div>
                    <div :key="row" class="flex flex-row" v-for="row in Object.keys(seatStructure)">
                        <span 
                            @click="selectSeat(row, seat[0])"
                            v-for="seat in seatStructure[row]" 
                            class="w-7 h-8 m-2 rounded-[2px]" 
                            :class="seat[1] == 'Available' ? 'bg-green-300' : seat[1] ==='Selected' ? 'bg-green-600' : 'bg-gray-300'"
                        ></span>
                    </div>
                </div>
            </div>

            <div v-else-if="currentStep === 4">
                
                <div class="w-full flex items-center flex-col mt-7">
                    <h1 class="text-[110px]">🍿</h1>
                    <h2 class="font-medium text-xl mt-7 text-gray-900">Enjoy the Movie!</h2>
                </div>
            </div>
        </div>

        <div class="flex flex-row mt-6 space-x-3">
            <Button 
                size="lg" 
                variant="subtle" 
                v-if="currentStep !== 0 && currentStep != 4" 
                @click="currentStep--"
            >Go Back</Button>
    
            <Button 
                size="lg"
                variant="solid" 
                v-if="currentStep !== 0 && currentStep != 4"
                @click="currentStep++"
            >Next</Button>
        </div>

    </div>
</template>

<script setup>
import { Button, Input } from "frappe-ui"
import { reactive, ref } from "vue";

function getSeatStructure(alphabets, numbers) {
    const structure = {}
    for (const alphabet of alphabets) {
        structure[alphabet] = []
        
        for (const number of numbers) {
            structure[alphabet].push([number, 'Available'])
        }
    }
    return structure
}

const seatStructure = reactive(getSeatStructure(
    ['A', 'B', 'C', 'D', 'E'], 
    [1, 2, 3, 4, 5, 6, 7, 8]
))




const today = new Date().toISOString().substring(0, 10)
const currentStep = ref(3);
const bookingData = reactive({
    numberOfSeats: 0,
    selectedSeats: [],
    date: today
});

function setNumberOfSeats(n) {
    bookingData.numberOfSeats = n
}

function selectSeat(row, number) {
    const seat = seatStructure[row].find((seat) => seat[0] === number)
    seat[1] = 'Selected'
    bookingData.selectedSeats.push(`${row}${number}`)
}
</script>