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