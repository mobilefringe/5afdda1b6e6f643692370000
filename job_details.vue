<template>
    <div> <!-- Without an outer container div this component template will not render -->
        <loading-spinner v-if="!dataLoaded"></loading-spinner>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="inside_header_background" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
                    <div class="main_container">
                        <h2>Jobs</h2>
                    </div>
                </div>
                <div class="main_container mobile_padding margin_30">
                    <div class="details_row">
                        <div class="details_col_3 hidden_phone">
                            <img class="img_max" src="//codecloud.cdn.speedyrails.net/sites/5afdda1b6e6f643692370000/image/jpeg/1527708236000/Casas Adobes - Aerial 01.jpg" alt="" />    
                        </div>
                        <div class="details_col_9" v-if="currentJob">
                            <router-link to="/jobs">
                                <div class="inside_page_header"><i class="fa fa-caret-left"></i> Back to List</div>
                            </router-link>
                            <h3 class="promo_name">{{ currentJob.name }}</h3>
                            <p class="promo_store_name">
                                <router-link v-if="currentJob.jobable_type == 'Store'" :to="'/stores/'+ currentJob.store.slug">
                                    {{ currentJob.store.name }}
                                </router-link>
                                <span v-else>{{ property.name }}</span>
                                <span>| </span>
                                <span v-if="isMultiDay(currentJob)" class="promo_date">{{ currentJob.start_date | moment("MMMM D", timezone)}} to {{ currentJob.end_date | moment("MMMM D", timezone)}}</span>
                                <span v-else class="promo_date">{{ currentJob.start_date | moment("MMMM D", timezone)}}</span>
                            </p>
                            <div class="promo_desc" v-html="currentJob.rich_description"></div>
                            <social-sharing v-if="currentJob" :url="shareURL(currentJob.slug)" :title="currentJob.title" :description="currentJob.body" :quote="truncate(currentJob.body)" :twitter-user="siteInfo.twitterHandle" :media="currentJob.image_url" inline-template>
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
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var n=arguments[t];for(var r in n)Object.prototype.hasOwnProperty.call(n,r)&&(e[r]=n[r])}return e};define(["Vue","vuex","moment","moment-timezone","vue-moment","vue-lazy-load","vue-social-sharing","json!site.json"],function(e,t,n,r,o,a,i,s){return e.use(a),e.component("social-sharing",i),e.component("job-details-component",{template:template,props:["id"],data:function(){return{dataLoaded:!1,pageBanner:"",currentJob:null,siteInfo:s}},created:function(){var e=this;this.$store.dispatch("getData","repos").then(function(){var t=e.findRepoByName("Jobs Banner");t&&(e.pageBanner=t.images[0])},function(){console.error("Could not retrieve data from server. Please check internet connection and try again.")}),this.$store.dispatch("getData","jobs").then(function(){e.currentJob=e.findJobBySlug(e.id),(null===e.currentJob||void 0===e.currentJob)&&e.$router.replace({path:"/jobs"}),e.dataLoaded=!0},function(){console.error("Could not retrieve data from server. Please check internet connection and try again.")})},computed:_extends({},t.mapGetters(["property","timezone","findRepoByName","findJobBySlug"])),methods:{isMultiDay:function(e){var t=this.timezone,r=n(e.start_date).tz(t).format("MM-DD-YYYY"),o=n(e.end_date).tz(t).format("MM-DD-YYYY");return r===o?!1:!0},truncate:function c(e){var c=_.truncate(e,{length:99,separator:" "});return c},shareURL:function(){var e=window.location.href;return e}}})});
</script>