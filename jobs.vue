<template>
    <div> <!-- without an outer container div this component template will not render -->
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
                            <img class="img_max" src="//codecloud.cdn.speedyrails.net/sites/5afdda1b6e6f643692370000/image/jpeg/1527708236000/Casas Adobes - 16.jpg" alt="" />    
                        </div>
                        <div class="details_col_9">
                            <!-- JOB -->
                            <b-card no-body class="mb-1 inside_page_toggle">
                                <b-card-header header-tag="header" class="p-1" role="tab">
                                    <b-btn block @click="toggleJobs = !toggleJobs" :aria-expanded="toggleJobs ? 'true' : 'false'" aria-controls="toggleJobs">
                                        Jobs
                                        <i v-if="toggleJobs"  class="fa fa-minus f"></i>
                                        <i v-else  class="fa fa-plus"></i>
                                    </b-btn>
                                </b-card-header>
                                <b-collapse v-if="jobList.length >= 1" v-for="job in jobList" v-model="toggleJobs" role="tabpanel" id="toggleJobs" class="accordion_body">
                                    <b-card-body>
                                        <div class="row">
                                            <div class="col-md-12">
                                                <h3 class="promo_name">{{job.name}}</h3>
                                                <p class="promo_store_name">
                                                    <router-link v-if="job.jobable_type == 'Store'" :to="'/stores/'+ job.store.slug">
                                                        {{ job.store.name }}
                                                    </router-link>
                                                    <span v-else>{{ property.name }}</span>
                                                    <span>| </span>
                                                    <span v-if="isMultiDay(job)" class="promo_date">{{ job.start_date | moment("MMMM D", timezone)}} to {{ job.end_date | moment("MMMM D", timezone)}}</span>
                                                    <span v-else class="promo_date">{{ job.start_date | moment("MMMM D", timezone)}}</span>
                                                </p>
                                                <div class="promo_desc" v-html="job.description_short"></div>
                                                <router-link :to="'/jobs/'+ job.slug" >
						                            <i class="fa fa-caret-right"></i> <a class="read_more">View Job Details</a>
				                                </router-link>
                                            </div>
                                        </div>
                                        <hr class="promo_separator" />
                                    </b-card-body>
                                </b-collapse>
                                <b-collapse v-if="jobList.length == 0" v-model="toggleJobs" role="tabpanel" id="toggleJobs" class="accordion_body">
                                    <b-card-body>
                                        <div class="row">
                                            <div class="col-md-12">
                                                <p>Sorry, there are no Jobs posted at this time. Please check back soon!</p>
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
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var n=arguments[t];for(var r in n)Object.prototype.hasOwnProperty.call(n,r)&&(e[r]=n[r])}return e};define(["Vue","vuex","moment","moment-timezone","vue-moment","bootstrap-vue"],function(e,t,n,r,o,a){return e.use(a),e.component("jobs-component",{template:template,data:function(){return{dataLoaded:!1,pageBanner:"",toggleJobs:!1}},created:function(){var e=this;this.loadData().then(function(){var t=e.findRepoByName("Jobs Banner");t&&(e.pageBanner=t.images[0]),e.dataLoaded=!0})},computed:_extends({},t.mapGetters(["property","timezone","findRepoByName","processedJobs"]),{jobList:function s(){console.log(this.processedJobs);var s=this.processedJobs,e=[];_.forEach(s,function(t){var r=n.tz(this.timezone).format(),o=n.tz(t.show_on_web_date,this.timezone).format();r>=o&&(t.description_short=_.truncate(t.description,{length:100,separator:" "}),e.push(t))});var t=_.orderBy(e,function(e){return e.end_date});return t.length>0&&(this.toggleJobs=!0),t}}),methods:{loadData:function(){var e;return regeneratorRuntime.async(function(t){for(;;)switch(t.prev=t.next){case 0:return t.prev=0,t.next=3,regeneratorRuntime.awrap(Promise.all([this.$store.dispatch("getData","repos"),this.$store.dispatch("getData","jobs")]));case 3:return e=t.sent,t.abrupt("return",e);case 7:t.prev=7,t.t0=t["catch"](0),console.log("Error loading data: "+t.t0.message);case 10:case"end":return t.stop()}},null,this,[[0,7]])},isMultiDay:function(e){var t=this.timezone,r=n(e.start_date).tz(t).format("MM-DD-YYYY"),o=n(e.end_date).tz(t).format("MM-DD-YYYY");return r===o?!1:!0}}})});
</script>