<template>
 
  <div>
     <Header/>
      <MobileNav/>
        <HeroSection v-on:listenerKeyword="listenerKeyword" v-on:listenerCategory="listenerCategory" />
          <PropertyCities/>
            <Services/>
            
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
   

      <!-- <div class="form-group">
        <input @keyup="search" v-model="key" type="text" class="form-control" placeholder="search">
      </div>
      <div class="row">

          <div v-for="result in results" :key="result.id" class="col-md-4">
            <div class="card card-body">

                <h4>{{result.title}}</h4>

            </div>
          </div>

      </div> -->
    <!-- <div :class="searchOn"  class="norm col-md-10 mx-auto">

        <div v-if="show" class="row">
          <div v-for="listing in listings" :key="listing.id"  class="col-md-4 mx-auto">
            <div class="card card-body">
              <h4>{{listing.title}}</h4>
              <p>{{listing.description}}</p>

              <p>{{listing.images.img_path}}</p>

               <router-link :to="{name: 'about', params:{slug: listing.slug}}" >view property</router-link>


            </div>
          </div>
        </div>

        <div v-else class="h-100">

          loading...

        </div>


    </div> -->


    <!-- // SERACH -->


                <div class="">

            <section id="feature-property" class="feature-property bgc-f7 pb30">
                <div class="container">
                    <div class="row">
                        <div class="col-lg-6 offset-lg-3">
                            <div class="main-title text-center">
                                <h2>Most Searched For</h2>
                                <p>Handpicked properties by our team.</p>
                            </div>
                        </div>
                    </div>
                    <div v-if="show"  class="row">

                    

                            <div v-for="listing in listings" :key="listing.id" class="col-md-6 col-lg-4">
                                <div class="feat_property home7">
                                    <div class="thumb">
                                        <img style="width: 230px; height: 230px;" class="img-whp" :src="getImage(listing.images[1])" alt="fp1.jpg">
                                        <div class="thmb_cntnt">
                                            <ul class="tag mb0">
                                                <li class="list-inline-item"><a href="#">{{listing.categories.name}}</a></li>
                                                
                                            </ul>
                                            <ul class="icon mb0">
                                                <li class="list-inline-item"><a href="#"><span class="flaticon-transfer-1"></span></a></li>
                                                <li class="list-inline-item"><a href="#"><span class="flaticon-heart"></span></a></li>
                                            </ul>
                                            <a class="fp_price" href="#">NGN {{listing.price}}</a>
                                        </div>
                                    </div>
                                    <div class="details">
                                        <div class="tc_content">
                                        
                                            
                                            <a href="">
                                            <h4>{{listing.title}}</h4>
                                            </a>
                                            <p class="text-secondary">{{listing.description}}</p>
                                            <p><span class="flaticon-placeholder"></span> {{listing.location}}</p>
                                            <ul class="prop_details mb0">
                                                  <li class="list-inline-item"><a href="#">Beds: {{listing.bedrooms}}</a></li>
                                                  <li class="list-inline-item"><a href="#">Baths: {{listing.bathrooms}}</a></li>
                                                  <li class="list-inline-item"><a href="#">Total Area: {{listing.total_area}}</a></li>
                                            </ul>
                                        </div>
                                        <div class="fp_footer">
                                            <ul class="fp_meta float-left mb0">
                                                <li class="list-inline-item"><a href="#"><img width="60" height="60" :src="getAvatar(listing.agents.avatar)"  alt="pposter1.png"></a></li>
                                                <li class="list-inline-item"><a href="#">name</a></li>
                                            </ul>
                                            <div class="fp_pdate float-right">time</div>
                                        </div>
                                    </div>
                                </div>
                            </div>	

                           				


                    </div>

                    <div v-else class="loading">

                      <h2 class="text-center mt-3">Loading...</h2>
                    </div>
                </div>

                <div class="col-lg-12 pb-2">
                    <div class="mx-auto text-center">
                    <a href="" class=" btn btn-lg btn-primary shadow">Show More</a>
                    </div>
                </div>
        </section>

    </div>
        <!-- <FeaturedProperty/> -->

    
  </div>
  
  
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import Header from "@/components/Header.vue";
import MobileNav from "@/components/MobileNav.vue";
import HeroSection from "@/components/HeroSection.vue";
import PropertyCities from "@/components/PropertyCities.vue";
import Services from "@/components/Services.vue";
import MostSearched from "@/components/MostSearched.vue";
// import FeaturedProperty from "@/components/FeaturedProperty.vue";
import axios from 'axios'

export default {
  watch: {
    
  },
  
  data () {
    return {
      listings: [],
       results: [],
      show: false,
      key: '',
      searchOn: '',
      keyword:'',
      category: '',
    }
  },
  methods: {

    listenerCategory(reply){

        this.keyword = reply;

        console.log('from keyword '+this.keyword)

    },
    listenerKeyword(reply){

        this.category = reply;

        console.log('from category '+this.category)

    },
    search(){

      

      console.log(this.key)

      this.searchOn = 'd-none'

         axios.post(process.env.VUE_APP_ROOT_API +'search', {
            key: this.key
          })
           .then((response)=>(
             
             this.results = response.data

            // console.log(response.data)
            
             
             
             ))
          .catch(function (error) {
            console.log(error);
          })
          .then(function () {
            // always executed
            
          }); 

    },
    load_listings(){
      this.show = true

      axios.get(process.env.VUE_APP_ROOT_API +'all_listings', {
            params: {
              ID: 12345
            }
          })
           .then((response)=>(
             
             this.listings = response.data
               
  
             ))
          .catch(function (error) {
            console.log(error);
          })
          .then(function () {
            // always executed
             this.show = false
          }); 

    },

     getImage(imgUrl){         

         return process.env.VUE_APP_PUBLIC_URL +"listing_images/" +imgUrl.img_path;


      },

       getAvatar(imgUrl){         

         return process.env.VUE_APP_PUBLIC_URL +"avatars/" +imgUrl.img_path;


      },

  },
  props: {
  },
  created () {
    
    

    setInterval(this.load_listings, 30000)



    
    
    

  },
  name: "home",
  components: {
    HelloWorld,
    Header,
    MobileNav,
    HeroSection,
    PropertyCities,
    Services,
    MostSearched,
    // FeaturedProperty
  }
};
</script>
