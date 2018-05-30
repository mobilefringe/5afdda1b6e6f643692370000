<template>
    <div> <!-- Without an outer container div this component template will not render -->
        <loading-spinner v-if="!dataLoaded"></loading-spinner>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="inside_header_background" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
                    <div class="main_container">
                        <h2>Promotions</h2>
                    </div>
                </div>
                <div class="main_container margin_30">
                    <div class="details_row">
                        <div class="details_col_3 hidden_phone">
                            <img class="img_max" src="//codecloud.cdn.speedyrails.net/sites/5afdda1b6e6f643692370000/image/jpeg/1527708236000/Casas Adobes - 58.jpg" alt="" />    
                        </div>
                        <div class="details_col_9" v-if="currentPromo">
                            <router-link to="/promotions-and-events">
                                <div class="inside_page_header"><i class="fa fa-caret-left"></i> Back to List</div>
                            </router-link>
                            <img v-lazy="currentPromo.image_url" :alt="'Promotion: ' + currentPromo.name" class="margin_20 img_max"/>
                            <h3 class="promo_name">{{ currentPromo.name }}</h3>
                            <p class="promo_store_name">
                                <router-link v-if="currentPromo.promotionable_type == 'Store'" :to="'/stores/'+ currentPromo.store.slug">{{ currentPromo.store.name }}</router-link>
                                <span v-else>{{ property.name }}</span>
                                <span>| </span>
                                <span v-if="isMultiDay(currentPromo)" class="promo_date">{{ currentPromo.start_date | moment("MMMM D", timezone)}} to {{ currentPromo.end_date | moment("MMMM D", timezone)}}</span>
                                <span v-else class="promo_date">{{ currentPromo.start_date | moment("MMMM D", timezone)}}</span>
                            </p>
                            <div class="promo_desc" v-html="currentPromo.rich_description"></div>
                            <social-sharing v-if="currentPromo" :url="shareURL(currentPromo.slug)" :title="currentPromo.title" :description="currentPromo.body" :quote="truncate(currentPromo.body)" :twitter-user="siteInfo.twitterHandle" :media="currentPromo.image_url" inline-template>
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
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var r=arguments[t];for(var o in r)Object.prototype.hasOwnProperty.call(r,o)&&(e[o]=r[o])}return e};define(["Vue","vuex","moment","moment-timezone","vue-moment","vue-lazy-load","vue-social-sharing","json!site.json"],function(e,t,r,o,n,a,i,s){return e.use(a),e.component("social-sharing",i),e.component("promo-details-component",{template:template,props:["id"],data:function(){return{dataLoaded:!1,pageBanner:"",currentPromo:null,siteInfo:s}},created:function(){var e=this;this.$store.dispatch("getData","repos").then(function(){var t=e.findRepoByName("Promos & Events Banner");t&&(e.pageBanner=t.images[0])},function(){console.error("Could not retrieve data from server. Please check internet connection and try again.")}),this.$store.dispatch("getData","promotions").then(function(){e.currentPromo=e.findPromoBySlug(e.id),(null===e.currentPromo||void 0===e.currentPromo)&&e.$router.replace({path:"/promotions"}),e.dataLoaded=!0},function(){console.error("Could not retrieve data from server. Please check internet connection and try again.")})},watch:{currentPromo:function(){null!=this.currentPromo&&("Store"===this.currentPromo.promotionable_type?_.includes(this.currentPromo.promo_image_url_abs,"missing")&&(this.currentPromo.image_url=this.currentPromo.store.store_front_url_abs):_.includes(this.currentPromo.promo_image_url_abs,"missing")&&(this.currentPromo.image_url="http://placehold.it/400x400"))}},computed:_extends({},t.mapGetters(["property","timezone","findRepoByName","findPromoBySlug"])),methods:{isMultiDay:function(e){var t=this.timezone,o=r(e.start_date).tz(t).format("MM-DD-YYYY"),n=r(e.end_date).tz(t).format("MM-DD-YYYY");return o===n?!1:!0},truncate:function u(e){var u=_.truncate(e,{length:99,separator:" "});return u},shareURL:function(){var e=window.location.href;return e}}})});
</script>