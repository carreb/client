<template>
    <div>
        <h3>{{ this.connectedUsers }} users watching. <br>Updates around every minute</h3>
        <div ref="bombDisplay" v-if="this.bombs.length > 0">
            <div ref="CombatBombs" v-if="arrayIsNotEmpty(getBombsOfSpecificType('Combat XP Bomb'))" class="bombBox">
                <h1>Combat XP Bombs</h1>
                <div v-for="bomb in getBombsOfSpecificType('Combat XP Bomb')" :key="bomb._id">
                    WC{{ bomb.world }}, by {{ bomb.throwerName }}. {{ timeUntilExpiry(bomb) }}
                </div>
            </div>
            <div ref="ProfXpBombs" v-if="arrayIsNotEmpty(getBombsOfSpecificType('Profession XP Bomb'))" class="bombBox">
                <h1>Profession XP Bombs</h1>
                <div v-for="bomb in getBombsOfSpecificType('Profession XP Bomb')" :key="bomb._id">
                    WC{{ bomb.world }}, by {{ bomb.throwerName }}. {{ timeUntilExpiry(bomb) }}
                </div>
            </div>
            <div ref="ProfSpeedBombs" v-if="arrayIsNotEmpty(getBombsOfSpecificType('Profession Speed Bomb'))" class="bombBox">
                <h1>Profession Speed Bombs</h1>
                <div v-for="bomb in getBombsOfSpecificType('Profession Speed Bomb')" :key="bomb._id">
                    WC{{ bomb.world }}, by {{ bomb.throwerName }}. {{ timeUntilExpiry(bomb) }}
                </div>
            </div>
            <div ref="LootBombs" v-if="arrayIsNotEmpty(getBombsOfSpecificType('Loot Bomb'))" class="bombBox">
                <h1>Loot Bombs</h1>
                <div v-for="bomb in getBombsOfSpecificType('Loot Bomb')" :key="bomb._id">
                    WC{{ bomb.world }}, by {{ bomb.throwerName }}. {{ timeUntilExpiry(bomb) }}
                </div>
            </div>
            <div ref="DungeonBombs"  v-if="arrayIsNotEmpty(getBombsOfSpecificType('Dungeon Bomb'))" class="bombBox">
                <h1>Dungeon Bombs</h1>
                <div v-for="bomb in getBombsOfSpecificType('Dungeon Bomb')" :key="bomb._id">
                    WC{{ bomb.world }}, by {{ bomb.throwerName }}. {{ timeUntilExpiry(bomb) }}
                </div>
            </div>
        </div>
        <div v-else>
            <h1>Looks like there are no active bombs :(</h1>
        </div>
    </div>
</template>


<script>
import io from "socket.io-client";
export default {
    name: 'bombList',
    data() {
        return {
            socket: {},
            ctx: {},
            bombs: {},
            connectedUsers: 0
        }
    },
    created() {
        this.socket = io("http://localhost:1313")
    },
    mounted() {
        this.socket.on("bombs", data => {
            console.log(data)
            this.bombs = data
        })
        this.socket.on("updateConnectedUsers", data => {
            this.connectedUsers = data
        })
    },
    methods: {
        getBombsOfSpecificType(specifiedBombType) {
            let bombArray = this.bombs
            let newBombArray = []
            for (let i = 0; i < bombArray.length; i++) {
                if (bombArray[i].bombType == specifiedBombType) {
                    newBombArray.push(bombArray[i])
                }
            }
            return newBombArray
        },
        timeUntilExpiry(bomb) {
            let throwTime = Math.floor(new Date(bomb.thrownAt).getTime()) / 1000
            let expirationTime = new Date(throwTime + bomb.ttl).getTime() * 1000
            expirationTime = new Date(expirationTime)
            let hour = expirationTime.getHours()
            let minute = expirationTime.getMinutes()
            if (minute < 10) {
                minute = `0${minute}`
            }
            return `Expires at ${hour}:${minute}`
        }
    },
    computed: {
        arrayIsNotEmpty() {
            return function(array) {
                if (array.length != 0) {
                    return true
                }
                return false
            }
        }
    }
}
</script>




<style scoped>
.bombBox {
        background-color: #313030;
        color: #fff;
        float: left;
        padding-top: 50px;
        border-radius: 10px;
        box-shadow: #000 5px 5px;
        padding-bottom: 50px;
        height: fit-content;
        width: 500px;
        margin: 20px;
    }
</style>