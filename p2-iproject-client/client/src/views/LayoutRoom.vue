<script>
import { mapActions , mapState } from 'pinia';
import { useMovieStore } from '../stores/movie';
    export default{
        name: 'LayoutRoom', 
        data(){
            return {
                id: this.$route.params.id, 
                seatNumber: [], 
                dateToday: '',
                baris: ["A", "B", "C", "D","E","F", "G", "H","I","J"], 
                totalPrice: ''
            }
        },
        created(){
          this.readTheaterById(1)
          this.getDateToday()
          this.getTotalPrice()
        
        },
        computed:{
          ...mapState(useMovieStore , ['theater'])
        },
        methods: {
            ...mapActions(useMovieStore , ['createTicket', 'readTheaterById' , 'tokenMidTrans']),
              getSeat(id){
                this.seatNumber.push(id)
              },

              getTotalPrice(){
                this.totalPrice = this.seatNumber.length * 40000
              }, 

              getDateToday(){
                const date = new Date();

                let day = date.getDate();
                let month = date.getMonth() + 1;
                let year = date.getFullYear();

                this.dateToday = `${day}-${month}-${year}`;
             
              }
              
        }
    }
</script>

<template>
  <div class="container p-3 bg-light mb-3 content">
<section id="kursi">
  <div class="atas">
    <p><img src="../assets/seat_free.png"> Available <img src="../assets/seat_own_booking.png"> Your Seats <img src="../assets/seat_other_book.png"> On Booking <img src="../assets/seat_booked.png"> Sold</p>
  </div>
  <hr>
  <div class="desc">
    <h5>{{ theater[1][0].title }}</h5>
    <p >Seats : {{ seatNumber }}</p>
    <p>Tickets 0/1</p>
    <p>{{ theater[1].name }}</p>
    <P>Studio 2</P>
    <p> Time {{ dateToday }} {{ theater[1][0].schedule[0].time }}</p>
    <p>Total Payment Rp.{{ totalPrice }}</p>
  </div>
  <hr>
  <div class="bawah d-flex justify-content-center">
    <div>
      <form>
        
      </form>
    <div class="baris" v-for="el in baris">
      <img v-for="(col , index) in 10" @click.prevent="getSeat(`${el}${index+1}`)"  class="ms-1" src="../assets/seat_free.png"> 
    </div>
    <div class="layar text-center p-2">
      <h4>SCREEN</h4>
    </div>
      <button @click.prevent="tokenMidTrans">Pembayaran</button>
    </div>
  </div>
</section>
</div>
</template>