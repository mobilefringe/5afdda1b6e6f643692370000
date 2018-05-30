<template>
    <div> <!-- Without an outer container div this component template will not render -->
        <loading-spinner v-if="!dataLoaded"></loading-spinner>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="inside_header_background" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
                    <div class="main_container">
                        <h2>Events</h2>
                    </div>
                </div>
                <div class="main_container mobile_padding margin_30">
                    <div class="details_row">
                        <div class="details_col_3 hidden_phone">
                            <img class="img_max" src="//codecloud.cdn.speedyrails.net/sites/5afdda1b6e6f643692370000/image/jpeg/1527708236000/Casas Adobes - 58.jpg" alt="" />    
                        </div>
                        <div class="details_col_9" v-if="currentEvent">
                            <router-link to="/promotions-and-events">
                                <div class="inside_page_header"><i class="fa fa-caret-left"></i> Back to List</div>
                            </router-link>
                            <img v-lazy="currentEvent.image_url" :alt="'Event: ' + currentEvent.name" class="margin_20 img_max"/>
                            <h3 class="promo_name">{{ currentEvent.name }}</h3>
                            <p class="promo_store_name">
                                <router-link v-if="currentEvent.eventable_type == 'Store'" :to="'/stores/'+ currentEvent.store.slug">
                                    {{ currentEvent.store.name }}
                                </router-link>
                                <span v-else>{{ property.name }}</span>
                                <span>| </span>
                                <span v-if="isMultiDay(currentEvent)" class="promo_date">{{ currentEvent.start_date | moment("MMMM D", timezone)}} to {{ currentEvent.end_date | moment("MMMM D", timezone)}}</span>
                                <span v-else class="promo_date">{{ currentEvent.start_date | moment("MMMM D", timezone)}}</span>
                            </p>
                            <div class="promo_desc" v-html="currentEvent.rich_description"></div>
                            <social-sharing v-if="currentEvent" :url="shareURL(currentEvent.slug)" :title="currentEvent.title" :description="currentEvent.body" :quote="truncate(currentEvent.body)" :twitter-user="siteInfo.twitterHandle" :media="currentEvent.image_url" inline-template>
                                <div class="social_share">
                                    <p>Share</p>
                                    <network network="facebook">
                                        <i class="fab fa-facebook"></i>
                                    </network>
                                    <network network="twitter">
                                        <i class="fab fa-twitter"></i>
                                    </network>
                                </div>
                            </social-sharing>
                        </div>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var n=arguments[t];for(var r in n)Object.prototype.hasOwnProperty.call(n,r)&&(e[r]=n[r])}return e};define(["Vue","vuex","moment","moment-timezone","vue-moment","vue-lazy-load","vue-social-sharing","json!site.json"],function(e,t,n,r,a,o,i,s){return e.use(o),e.component("social-sharing",i),e.component("event-details-component",{template:template,props:["id"],data:function(){return{dataLoaded:!1,pageBanner:"",currentEvent:null,siteInfo:s}},created:function(){var e=this;this.$store.dispatch("getData","repos").then(function(){var t=e.findRepoByName("Promos & Events Banner");t&&(e.pageBanner=t.images[0])},function(){console.error("Could not retrieve data from server. Please check internet connection and try again.")}),this.$store.dispatch("getData","events").then(function(){e.currentEvent=e.findEventBySlug(e.id),(null===e.currentEvent||void 0===e.currentEvent)&&e.$router.replace({name:"404"}),e.dataLoaded=!0},function(){console.error("Could not retrieve data from server. Please check internet connection and try again.")})},watch:{currentEvent:function(){null!=this.currentEvent&&("Store"===this.currentEvent.eventable_type?_.includes(this.currentEvent.event_image_url_abs,"missing")&&(this.currentEvent.image_url=this.currentEvent.store.store_front_url_abs):_.includes(this.currentEvent.event_image_url_abs,"missing")&&(this.currentEvent.image_url="http://placehold.it/400x400"))}},computed:_extends({},t.mapGetters(["property","timezone","findRepoByName","processedEvents","findEventBySlug"])),methods:{isMultiDay:function(e){var t=this.timezone,r=n(e.start_date).tz(t).format("MM-DD-YYYY"),a=n(e.end_date).tz(t).format("MM-DD-YYYY");return r===a?!1:!0},truncate:function u(e){var u=_.truncate(e,{length:99,separator:" "});return u},shareURL:function(){var e=window.location.href;return e}}})});
</script>