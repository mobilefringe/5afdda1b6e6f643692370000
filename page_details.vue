<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loading-spinner v-if="!dataLoaded"></loading-spinner>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="inside_header_background" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
                    <div class="main_container">
                        <h2 v-html="currentPage.title"></h2>
                    </div>
                </div>
                <div class="main_container mobile_padding margin_30">
                    <div class="details_row">
                        <div class="details_col_3 hidden_phone">
                            <img class="img_max" src="//codecloud.cdn.speedyrails.net/sites/5afdda1b6e6f643692370000/image/jpeg/1527708236000/Casas Adobes - Aerial 03.jpg" alt="" />    
                        </div>
                        <div class="details_col_9">
                            <div class="page_body" v-html="currentPage.body"></div>
                        </div>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var a=arguments[t];for(var r in a)Object.prototype.hasOwnProperty.call(a,r)&&(e[r]=a[r])}return e};define(["Vue","vuex"],function(e,t){return e.component("page-details-component",{template:template,props:["id"],data:function(){return{dataLoaded:!1,pageBanner:"",currentPage:null}},created:function(){var e=this;this.loadData().then(function(){var t=e.findRepoByName("Inside Page Banner");t&&(e.pageBanner=t.images[0]),e.updateCurrentPage(e.id),e.dataLoaded=!0})},watch:{$route:function(){this.updateCurrentPage(this.$route.params.id)}},computed:_extends({},t.mapGetters(["property","findRepoByName"])),methods:{loadData:function(){var e;return regeneratorRuntime.async(function(t){for(;;)switch(t.prev=t.next){case 0:return t.prev=0,t.next=3,regeneratorRuntime.awrap(Promise.all([this.$store.dispatch("getData","repos")]));case 3:return e=t.sent,t.abrupt("return",e);case 7:t.prev=7,t.t0=t["catch"](0),console.log("Error loading data: "+t.t0.message);case 10:case"end":return t.stop()}},null,this,[[0,7]])},updateCurrentPage:function(){var e=this;this.$store.dispatch("LOAD_PAGE_DATA",{url:this.property.mm_host+"/pages/"+this.id+".json"}).then(function(t){e.currentPage=t.data,e.dataLoaded=!0},function(){console.error("Could not retrieve data from server. Please check internet connection and try again."),e.$router.replace({name:"404"})})}}})});
</script>