<template>
<div class="v-application">

<div class="section-title d-flex align-start">
    <v-card-title  class="pa-0">Already have a phone? Choose your Power Plan</v-card-title>
    <div class="plans-filter" :class="isShow? 'show' : 'hide'" >
        <span>Show plans with:</span>
        <v-radio-group v-model="radioGroup" color="primary ma-0">
    
      <v-radio
        :label="'Flexi minutes (national & intl)'"
        
        color="secondary"
        @click="filterPlans(1)"
      ></v-radio>
      <v-radio
        :label="'National minutes'"
        
        color="secondary"
        @click="filterPlans(2)"
      ></v-radio>
    </v-radio-group>

    <!-- <button class="v-btn" @click="reInit()">Slick</button> -->
    
    <v-radio-group v-model="radioGroup1" color="primary">
    
      <v-radio
        :label="'12-month contract'"
        
        color="secondary"
        @click="filterContract(1)"
      ></v-radio>
      <v-radio
        :label="'No Contract'"
        
        color="secondary"
        @click="filterContract(2)"
      ></v-radio>
    </v-radio-group>
    
    </div>
    <v-btn @click="isShow = !isShow" outlined small color="primary">{{isShow ? "Hide Filters" : "Show Filters"}}</v-btn>
</div>

<div class="plans ">
    <Slick ref="slick" :options="slickOptions">
       
    <v-card class="mx-4 rounded-lg"  hover outlined v-for="(data, index) in planTypeFilter() " :key='index'>
       <div v-if="data.popular" class="most-popular">Most popular</div> 
        <div class="plan-amount">
            <span>You Pay</span>
            <v-card-title class="pa-0"> AED {{data.planAmount}} <small class="px-2">/month</small> </v-card-title>
            <v-card-text class="pa-0"> {{data.contract > 0 ? "For 12 months" : "No Contract"}} + 5% VAT </v-card-text>
        </div>
        <hr/>
        <div class="plan-name">
            <span>You Get</span>
            <v-card-title class="pa-0">Power Plan {{data.planName}}</v-card-title>
        </div>
        <hr/>
        <div class="data-n-mins">
            <div class="data d-flex align-baseline">
                <v-card-title class="pa-0" :class="data.dataOffer > 1 || data.dataOffer == 0 ? 'grey--text' : ''" > {{data.data}} </v-card-title>
                <v-card-title class="pa-0" v-if="data.dataOffer > 1"> {{data.data * data.dataOffer}}</v-card-title>
                <v-card-title class="pa-0" v-show="!data.dataOffer == 0">GB</v-card-title>
                <v-card-title class="pa-0" v-if="data.dataOffer == 0"> Unlimited </v-card-title>
                <span>{{data.dataOffer > 1 ? "Double" : ''}}  national data</span>
            </div>
            <div class="mins d-flex align-baseline" v-if="data.flexmins > 0"> 
                <v-card-title class="pa-0" :class="data.minsOffer == 0 ? 'grey--text' : '' " > {{data.flexmins}} </v-card-title>
                <v-card-title class="pa-0" v-if="data.minsOffer == 0"> Unlimited </v-card-title>
                <span>Flexi minutes</span>
            </div>
            <div class="mins d-flex align-baseline" v-else-if="data.natmins > 0"> 
                <v-card-title class="pa-0" :class="data.minsOffer == 0 ? 'grey--text' : '' " > {{data.natmins}} </v-card-title>
                <v-card-title class="pa-0" v-if="data.minsOffer == 0"> Unlimited </v-card-title>
                <span>National minutes</span>
            </div>
        </div>
        <hr/>
        <div class="details">
            <ul>
                <li v-for="detail in data.details" :key="detail">
                    {{detail}}
                </li>
            </ul>
        </div>
        
        <div class="limited-offer d-flex flex-column align-start">
            <div class="w-100" v-if="data.entertainOffer">
            <hr/>
            <v-chip small>Limited time Offer</v-chip>
            <div><strong>The Entertainer</strong> on us for 12 months</div>
        </div>
        </div>
 <hr/> 
        <div class="action-footer align-center d-flex justify-space-between">
              
            <a href="#">What you get</a>
            <v-btn outlined x-large color="primary">Select</v-btn>
        </div>
        
    </v-card>
    </Slick>
</div>
</div>
</template>

<script>
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'
import Slick from 'vue-slick';
import duplans from './plans.js'

export default {
  name: "Plans",
  components: { Slick },
  data(){
      return{
          isShow : true,
          planType : 1,
          contractType : 1,
            plansData: duplans,

          slickOptions: {
            "arrows": false,
            "dots": true,
            "infinite": false,
            "centerMode": false,
            "centerPadding": "0",
            "slidesToShow": 4,
            "speed": 500,
            "variableWidth": true,
            "slidesToScroll": 1,
            "initialSlide" : 0,
            "responsive": [
            {
            "breakpoint": 1665,
                "settings": {
                    "slidesToShow": 3,
                }
            },
            {
            "breakpoint": 1226,
                "settings": {
                    "slidesToShow": 2,
                }
            },
            {
            "breakpoint": 767,
                "settings": {
                    "slidesToShow": 1,
                }
            }
            ]
            },
            radioGroup: 0,
            radioGroup1: 0,
      }
  },
  methods:{
      
      
        filterPlans(planType){
           
            this.beforeUpdate();
            this.planType = planType;
            this.updated();


        },
        filterContract(contractType){
           
            this.beforeUpdate();
            this.contractType = contractType;
            this.updated();


        },


        planTypeFilter(){
            if(this.planType === 1 && this.contractType === 1){
                return this.plansData.filter((data) => data.flexmins > 0 && data.contract == 12);
            }
            else if(this.planType === 2 && this.contractType === 1){
                return this.plansData.filter((data) => data.natmins > 0 && data.contract == 12);
            }
            else if(this.planType === 1 && this.contractType === 2){
                return this.plansData.filter((data) => data.flexmins > 0 && data.contract == 0);
            }
            else if(this.planType === 2 && this.contractType === 2){
                return this.plansData.filter((data) => data.natmins > 0 && data.contract == 0);
            }
            // else if(this.contractType === 1){
            //     return this.plansData.filter((data) => data.contract == 12);
            // }
            // else if(this.contractType === 2){
            //     return this.plansData.filter((data) => data.contract == 0);
            // }
            
            
        },
        beforeUpdate() {
            console.log('beforeupdate');
        if (this.$refs.slick) {
            this.$refs.slick.destroy();
        }
    },
    updated() {
        console.log('updated');
        this.$nextTick(function () {
            if (this.$refs.slick) {
                this.$refs.slick.create(this.slickOptions);
            }
        });
    },
   
  }
}
</script>
<style scoped>

</style>
