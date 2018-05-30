<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loading-spinner v-if="!dataLoaded"></loading-spinner>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="inside_header_background" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
                    <div class="main_container">
                        <h2>Events & Promotions</h2>
                    </div>
                </div>
                <div class="main_container mobile_padding margin_30">
                    <div class="details_row">
                        <div class="details_col_3 hidden_phone">
                            <img class="img_max" src="//codecloud.cdn.speedyrails.net/sites/5afdda1b6e6f643692370000/image/jpeg/1527708236000/Casas Adobes - 23.jpg" alt="" />    
                        </div>
                        <div class="details_col_9">
                            <!-- PROMOTIONS -->
                            <b-card no-body class="mb-1 inside_page_toggle">
                                <b-card-header header-tag="header" class="p-1" role="tab">
                                    <b-btn block @click="togglePromos = !togglePromos" :aria-expanded="togglePromos ? 'true' : 'false'" aria-controls="togglePromotions">
                                        Promotions
                                        <i v-if="togglePromos"  class="fa fa-minus f"></i>
                                        <i v-else  class="fa fa-plus"></i>
                                    </b-btn>
                                </b-card-header>
                                <b-collapse v-if="promoList.length >= 1" v-for="promo in promoList" v-model="togglePromos" role="tabpanel" id="togglePromotions" class="accordion_body">
                                    <b-card-body>
                                        <div class="row">
                                            <div class="col-md-5" v-if="">
                                                <img :src="promo.image_url" :alt="'Promotion: ' + promo.name" class="max_img" />
                                            </div>
                                            <div class="col-md-7">
                                                <h3 class="promo_name">{{promo.name}}</h3>
                                                <p class="promo_store_name">
                                                    <router-link v-if="promo.promotionable_type == 'Store'" :to="'/stores/'+ promo.store.slug">
                                                        {{ promo.store.name }}
                                                    </router-link>
                                                    <span v-else>{{ property.name }}</span>
                                                    <span>| </span>
                                                    <span v-if="isMultiDay(promo)" class="promo_date">{{ promo.start_date | moment("MMMM D", timezone)}} to {{ promo.end_date | moment("MMMM D", timezone)}}</span>
                                                    <span v-else class="promo_date">{{ promo.start_date | moment("MMMM D", timezone)}}</span>
                                                </p>
                                                <div class="promo_desc" v-html="promo.description_short"></div>
                                                <router-link :to="'/promotions/'+ promo.slug" >
						                            <i class="fa fa-caret-right"></i> <span class="read_more">View Promotion Details</span>
				                                </router-link>
                                            </div>
                                        </div>
                                        <hr class="promo_separator" />
                                    </b-card-body>
                                </b-collapse>
                                <b-collapse v-if="promoList.length == 0" v-model="togglePromos" role="tabpanel" id="togglePromotions" class="accordion_body">
                                    <b-card-body>
                                        <div class="row">
                                            <div class="col-md-12">
                                                <p>Sorry, there are no Promotions posted at this time. Please check back soon!</p>
                                            </div>
                                        </div>
                                        <hr class="promo_separator" />
                                    </b-card-body>
                                </b-collapse>
                            </b-card>
                                                        <!-- EVENTS -->
                            <b-card no-body class="mb-1 inside_page_toggle">
                                <b-card-header header-tag="header" class="p-1" role="tab">
                                    <b-btn block @click="toggleEvents = !toggleEvents" :aria-expanded="toggleEvents ? 'true' : 'false'" aria-controls="toggleEvents">
                                        Events
                                        <i v-if="toggleEvents"  class="fa fa-minus f"></i>
                                        <i v-else  class="fa fa-plus"></i>
                                    </b-btn>
                                </b-card-header>
                                <b-collapse v-if="eventList.length >= 1" v-for="event in eventList" v-model="toggleEvents" role="tabpanel" id="toggleEvents" class="accordion_body">
                                    <b-card-body>
                                        <div class="row">
                                            <div class="col-md-5" v-if="">
                                                <img :src="event.image_url" :alt="'Event: ' + event.name" class="max_img" />
                                            </div>
                                            <div class="col-md-7">
                                                <h3 class="promo_name">{{event.name}}</h3>
                                                <p class="promo_store_name">
                                                    <router-link v-if="event.eventable_type == 'Store'" :to="'/stores/'+ event.store.slug">
                                                        {{ event.store.name }}
                                                    </router-link>
                                                    <span v-else>{{ property.name }}</span>
                                                    <span>| </span>
                                                    <span v-if="isMultiDay(event)" class="promo_date">{{ event.start_date | moment("MMMM D", timezone)}} to {{ event.end_date | moment("MMMM D", timezone)}}</span>
                                                    <span v-else class="promo_date">{{ event.start_date | moment("MMMM D", timezone)}}</span>
                                                </p>
                                                <div class="promo_desc" v-html="event.description_short"></div>
                                                <router-link :to="'/events/'+ event.slug" >
						                            <i class="fa fa-caret-right"></i> <span class="read_more">View Event Details</span>
				                                </router-link>
                                            </div>
                                        </div>
                                        <hr class="promo_separator" />
                                    </b-card-body>
                                </b-collapse>
                                <b-collapse v-if="eventList.length == 0" v-model="toggleEvents" role="tabpanel" id="toggleEvents" class="accordion_body">
                                    <b-card-body>
                                        <div class="row">
                                            <div class="col-md-12">
                                                <p>Sorry, there are no Events posted at this time. Please check back soon!</p>
                                            </div>
                                        </div>
                                        <hr class="promo_separator" />
                                    </b-card-body>
                                </b-collapse>
                            </b-card>
                        </div>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var r=arguments[t];for(var o in r)Object.prototype.hasOwnProperty.call(r,o)&&(e[o]=r[o])}return e};define(["Vue","vuex","moment","moment-timezone","vue-moment","vue-lazy-load","bootstrap-vue"],function(e,t,r,o,n,a,s){return e.use(s),e.use(a),e.component("promotions-and-events-component",{template:template,data:function(){return{dataLoaded:!1,pageBanner:"",toggleEvents:!1,togglePromos:!1}},created:function(){var e=this;this.loadData().then(function(){var t=e.findRepoByName("Promos & Events Banner");t&&(e.pageBanner=t.images[0]),e.dataLoaded=!0})},computed:_extends({},t.mapGetters(["property","timezone","findRepoByName","processedEvents","processedPromos"]),{eventList:function i(){var i=this.processedEvents,e=[];_.forEach(i,function(t){var o=r.tz(this.timezone).format(),n=r.tz(t.show_on_web_date,this.timezone).format();o>=n&&(null!=t.store&&void 0!=t.store&&_.includes(t.store.image_url,"missing")&&(t.store.image_url="http://placehold.it/400x400"),_.includes(t.image_url,"missing")&&(t.image_url="http://placehold.it/400x400"),t.description_short=_.truncate(t.description,{length:100,separator:" "}),e.push(t))});var t=_.orderBy(e,function(e){return e.end_date});return t.length>0&&(this.toggleEvents=!0),t},promoList:function(){var e=[];_.forEach(this.processedPromos,function(t){var o=r.tz(this.timezone).format(),n=r.tz(t.show_on_web_date,this.timezone).format();o>=n&&(null!=t.store&&void 0!=t.store&&_.includes(t.store.image_url,"missing")&&(t.store.image_url="http://placehold.it/400x400"),_.includes(t.image_url,"missing")&&(t.image_url="http://placehold.it/400x400"),t.description_short=_.truncate(t.description,{length:100,separator:" "}),e.push(t))});var t=_.orderBy(e,[function(e){return e.end_date}]);return t.length>0&&(this.togglePromos=!0),t}}),methods:{loadData:function(){var e;return regeneratorRuntime.async(function(t){for(;;)switch(t.prev=t.next){case 0:return t.prev=0,t.next=3,regeneratorRuntime.awrap(Promise.all([this.$store.dispatch("getData","repos"),this.$store.dispatch("getData","events"),this.$store.dispatch("getData","promotions")]));case 3:e=t.sent,t.next=9;break;case 6:t.prev=6,t.t0=t["catch"](0),console.log("Error loading data: "+t.t0.message);case 9:case"end":return t.stop()}},null,this,[[0,6]])},isMultiDay:function(e){var t=this.timezone,o=r(e.start_date).tz(t).format("MM-DD-YYYY"),n=r(e.end_date).tz(t).format("MM-DD-YYYY");return o===n?!1:!0}}})});
</script>
