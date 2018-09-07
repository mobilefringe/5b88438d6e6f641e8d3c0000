<template>
	<div>
		<div class="inside_page_header" v-if="pageBanner" v-bind:style="{ background: 'url(' + pageBanner.image_url + ') center center' }">
			<div class="page_container position_relative">
				<h2>Search Results</h2>
			</div>
		</div>
		<div class="page_container text-left" v-if="searchResults.length > 0" id="searchResults">
            <p class="search_result_title">Found {{searchResults.length}} results matching "{{searchQuery}}"</p>
			<div v-for="(result,index) in searchResults" :key="index" >
                <div class="row result_container_row">
                    
                    <div class="col-md-2">
                        <img :src="result.image_url"/>
                    </div>
                    <div class="col-md-10 search_result_content">
                        <h3>{{result.name}}</h3>
                        <p>{{truncated(result.description)}}</p>
                        <nuxt-link v-if="result.store_front_url_abs" class="result_link hvr-icon-forward" :to="{name: 'stores-slug', params:{slug:result.slug, category: result.category_name}}">Learn more about {{result.name}} <i class="fa fa-chevron-right hvr-icon"></i></nuxt-link>
                        <nuxt-link v-else class="result_link hvr-icon-forward" :to="{ name: 'events-and-promos-slug', params: { slug: result.slug }}">Learn more about {{result.name}} <i class="fa fa-chevron-right hvr-icon"></i></nuxt-link>

                    </div>
                </div>                
            </div>
		</div>
        <div class="page_container text-left" v-else> 
            <h3>Sorry, there are no search results. Please try a different query.</h3>
        </div>
	</div>
	
</template>

<script>
    define(["Vue", "vuex", "jquery", "vee-validate", "json!site.json"], function(Vue, Vuex, $, VeeValidate, site) {
        Vue.use(VeeValidate);
        return Vue.component("newsletter-component", {
            template: template, // the variable template will be injected
            props:['inside_banner'],
            data() {
                return {
                    searchResults: null,
                    searchQuery: null
                };
            },
            created() {
                var temp_repo = this.findRepoByName('Pages Banner');
                if(temp_repo !== null && temp_repo !== undefined) {
                       temp_repo = temp_repo.images;
                       this.pageBanner = temp_repo[0];
                    }
                    else {
                        this.pageBanner = {
                            "image_url": "//codecloud.cdn.speedyrails.net/sites/5b88438d6e6f641e8d3c0000/image/png/1531495616000/inside_banner.png"
                        }
                    }
            },
            beforeRouteUpdate(to, from, next) {
        this.$nextTick(function() {
            this.updateResults();
        });
        next();
    },
    created() {
        this.updateResults();
        if (
            this.$route.params.results == null &&
            this.$route.params.results == undefined
        ) {
            this.$router.push("/");
        }
    },
    computed: {
        ...mapGetters(["property", "findRepoByName"]),
        pageBanner() {
            var pageBanner = null;
            var temp_repo = this.findRepoByName("Pages Banner").images;
            if (temp_repo != null) {
                pageBanner = temp_repo[0];
            } else {
                pageBanner = {
                    image_url: "//codecloud.cdn.speedyrails.net/sites/5b5f2c136e6f644fcb5b0100/image/jpeg/1529532304000/insidebanner2.jpg"
                };
            }
            return pageBanner;
        }
    },
    methods: {
        truncated(string) {
            return _.truncate(string, {
                length: 150,
                separator: " "
            });
        },
        updateResults() {
            if (
                this.$route.query.searchQuery !== null &&
                this.$route.query.searchQuery !== undefined &&
                this.$route.query.searchQuery.length > 0
            ) {
                if (
                    this.$route.params.results !== null &&
                    this.$route.params.results !== undefined &&
                    Array.isArray(this.$route.params.results)
                ) {
                    this.searchResults = this.$route.params.results;
                    this.searchQuery = this.$route.query.searchQuery;
                }
            } else {
                this.$router.push("/");
            }
        }
    }
        });
    });
</script>