<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <button @click="test">TEST</button>
                <div class="container" v-show="!visible1" style="height: 222px;">
                    <div class="container">
                        <div class="row justify-content-center">
                            <h4>dealer ({{dealercount}})</h4>
                        </div>
                        <div class="row justify-content-center">
                            <div v-for="dealercard in dealercards">
                                <img style="height: 80px; width:66px; margin-left: 5px;" v-bind:src="dealercard.url" v-model="dealercards">
                            </div>
                        </div> 


                    </div>
                    <div class="container" style="padding-top: 100px;">
                        <div class="row justify-content-center">
                            <div v-for="usercard in usercards">
                                <img style="height: 80px; width:66px; margin-left: 5px;" v-bind:src="usercard.url" v-model="usercards">
                            </div>
                        </div>
                        <div <div class="row justify-content-center" style="padding-top: 10px;">
                            <h4>{{user}} ({{usercount}})</h4>                                                       
                        </div>                                              
                    </div>                       
                </div>                
                    <br>
                    <br>
            
                <div class="container" v-show="visible1">
                    <div class="row justify-content-center">
                        <div>
                            <p>Make your bet:</p>
                            <input type="number" name="bet" style="width:100px;"><br>
                            <input type="button" @click="visible1=!visible1" class="btn btn-danger" name="go" value="GO" style="width: 100px; margin-top: 10px;">
                        </div>
                    </div>
                </div>  
                    <br>
                    <br>
                    <br>

            <!-- Контейнер для вывода второй партии кнопок-->       
                <div v-show="!visible1">  
                    <div class="container" v-show="visible2" >
                        <div class="row justify-content-center" style="padding-left: 8%;">
                            <div class="col-sm">
                                <input type="button" @click="visible2=!visible2, addusercard()" class="btn btn-danger" name="hit" value="hit" style="width: 100px;">
                            </div>
                            <div class="col-sm">
                                <input type="button" @click="visible2=!visible2, adddealercard()" class="btn btn-danger" name="stop" value="stop" style="width: 100px;" >
                            </div>
                            <div class="col-sm">
                                <input type="button" @click="visible2=!visible2, addusercard(), double()" class="btn btn-danger" name="double" value="double" style="width: 100px;">
                            </div>
                        </div>
                    </div> 
                    <div class="container" v-show="!visible2">
                        <div class="row justify-content-center" style="padding-left: 10%; padding-right: 10%; margin-left:10%">
                            <div class="col">
                                <input type="button" @click="addusercard()" class="btn btn-danger" name="hit" value="hit" style="width: 100px;">
                            </div>
                            <div class="col">
                                <input type="button" @click="adddealercard()" class="btn btn-danger" name="stop" value="stop" style="width: 100px;" >
                            </div>                        
                        </div>
                    </div>
                </div>
                    <br>
                    <br>                
                <table class="table">
                    <tbody>
                        <tr v-for="card in cards">                            
                            <td>{{card.title}}</td>
                            <td><img style="height: 80px; width:66px" v-bind:src="card.url"></td>
                            <td>{{card.value}}</td>
                        </tr>
                    </tbody>
                </table>

            </div>
        </div>
    </div>
</template>

<script>
    export default {
        
        data() {
            return {
                visible1: true,
                visible2: true,
                dealercount: this.dealercards[0].value,                          //изначальные очки дилера
                usercount: this.usercards[0].value + this.usercards[1].value,    //изначальные очки игрока
                tempindex : null                                                 //для временного индекса массива
            }            
        }, 

        props: [
            'cards',
            'user',
            'usercards',
            'dealercards'
        ],

        methods: {
            test() {   //для тестов
                
                alert(Math.floor(Math.random()*5)+1);
                console.log(this.cards.length);
                console.log(this.cards);
                
            },

            addusercard() {
                this.tempindex = Math.floor(Math.random()*this.cards.length-1)+1;  // рандомное число из cards[]
                this.usercards.push(this.cards[this.tempindex]);                   // добавление в карты к игроку
                this.usercount+= this.cards[this.tempindex].value;                 // очки игрока после добавления карты
                this.cards.splice(this.tempindex, 1);                              // удаление из общей колоды

                if(this.usercount > 21){                           //результат с задержкой 50мсек для очерёдности событий
                    setTimeout( function(){
                       alert("You loose!");                                      
                   }, 50);
                }  
                                 
               console.log(this.usercards);
               console.log(this.cards);
            },

            adddealercard() {
                console.log(this.dealercards[0]);
            },

            double() {
                alert("doubleee!!!");
            }

        },  

        mounted() {
            console.log('Component mounted.')
        }

    }
</script>
