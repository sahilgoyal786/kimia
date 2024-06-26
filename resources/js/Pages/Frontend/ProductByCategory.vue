<script setup>
    import FrontendLayout from '@/Layouts/FrontendLayout.vue';
    import {Head} from '@inertiajs/vue3';
    defineProps({
        slug: {type: String},
        category: {type: String},
        products: {type: Object}
    });
    const pageRange = (currentPage,lastPage) => {
        const ranges = [];
        const pages = [];
        if((currentPage - 4) > 0 && currentPage == lastPage){
            ranges.push((currentPage - 3));
            ranges.push((currentPage - 2));
            ranges.push((currentPage - 1));
        }
        if((currentPage - 3) > 0 && (currentPage + 1) == lastPage){
            ranges.push((currentPage - 2));
            ranges.push((currentPage - 1));
        }
        if((currentPage - 2) > 0 && (currentPage + 2) == lastPage){
            ranges.push((currentPage - 1));
        }
        for(let i = currentPage;i <= lastPage;i++){
            ranges.push(i);
        }
        if(ranges.length > 4){
            pages.push(ranges[0]);
            pages.push(ranges[1]);
            pages.push('...');
            pages.push(ranges[ranges.length - 2]);
            pages.push(ranges[ranges.length - 1]);
        }else{
            for(let k = 0;k<ranges.length;k++){
                pages.push(ranges[k]);
            }
        }
        return pages;
    }
</script>
<template>
    <FrontendLayout>
        <Head>
            <title>{{category}} &#8211; Kimia Corp.</title>
            <meta name="description" content="Contact Kimia Corp. today for more information!">
        </Head>
        <section class="inner-banner" style="background-image: url('/assets/images/about-banner.jpg');">
            <div class="container flex text-center">
                <h1 class="mt-0 mb-0 row text-white text-capitalize">{{category}}</h1>       
            </div>
        </section>
        <section class="pad-100-15 category-section">
            <div v-if="slug == 'new'" class="container">
                <p class="text-center">Coming soon</p>
            </div>
            <div v-else-if="slug == 'pas'" class="container">
                <p class="text-center">The product line PAS is meticulously designed to expedite synthesis processes by eliminating preparatory steps, thereby saving valuable time. Beyond time efficiency, our products offer a host of advantages:</p>
                <div class="pas-content">
                    <ol>
                        <li>
                            <span class="large-icon"><img src="/assets/images/cost-effective-icon.png" width="35"/></span>
                            <strong>Cost-effectiveness</strong> Our products provide affordable access to a wide range of compounds, ensuring that cutting-edge research remains within budgetary constraints.
                        </li>
                        <li>   <span class="large-icon"><img src="/assets/images/eco-friendly-icon.png" width="30"/></span>
                            <strong>Eco-friendly</strong> With minimal solvent usage and waste generation, our products promote sustainable laboratory practices, contributing to a greener environment.
                        </li>
                        <li>  <span class="large-icon"><img src="/assets/images/space-efficiency-icon.png" width="30"/></span>
                            <strong>Space efficiency</strong> By requiring minimal storage space, our products optimize laboratory organization and resource management.
                        </li>
                        <li>  
                            <span class="large-icon"><img src="/assets/images/zero-wastage-icon.png" width="35"/></span>
                            <strong>Zero wastage</strong> Our formulations ensure that no compounds are left unused, maximizing efficiency and minimizing resource depletion.
                        </li>
                    </ol>
                </div>
                <p class="text-center">Browse through our catalogue to explore our diverse range of compounds, each available in a convenient 100 μmole scale, ready to catalyze your next breakthrough reaction.</p>
            </div>
            <div v-if="products" class="container flex gap-20">
                <div class="card product-card" v-for="product in products.data" :key="product.id">
                    <div class="product-feature-image flex text-center">
                        <a :href="route('productDetail',product.slug)"><img :src="product.image" :alt="product.catalog_number"></a>
                    </div>
                    <div class="product-content text-center">
                        <div class="sku mt-0 mb-1">{{product.catalog_number}}</div>
                        <h4 class="product-title mt-0 mb-1"><a :href="route('productDetail',product.slug)">{{product.name}}</a></h4>
                        <div class="btn-wrap">
                            <a class="btn secondary-btn" :href="route('productDetail',product.slug)"><span class="btn-text">View Detail</span></a>
                        </div>
                    </div>
                </div>
                <div class="row mt-5 text-center" v-if="((products.current_page > 1) || (products.current_page < products.last_page))">
                    <ul class="pagination flex gap-10 mt-0 mb-0 text-center">
                        <li>
                            <a v-if="products.current_page > 1" class="prev page-icon" :href="route('productDetail',{slug: slug,page: (products.current_page - 1)})"><i class="icon-left-open-big"></i></a>
                            <a v-else class="prev page-icon"><i class="icon-left-open-big"></i></a>
                        </li>
                        <li v-for="page in pageRange(products.current_page,products.last_page)" :key="page">
                            <a v-if="page == products.current_page" :class="((page == products.current_page) ? 'prev page-icon active' : 'prev page-icon')">{{page}}</a>
                            <a v-else-if="page != '...'" :class="((page == products.current_page) ? 'prev page-icon active' : 'prev page-icon')" :href="route('productDetail',{slug: slug,page: page})">{{page}}</a>
                            <a v-else class="prev page-icon">{{page}}</a>
                        </li>
                        <li>
                            <a v-if="products.current_page < products.last_page" class="next page-icon" :href="route('productDetail',{slug: slug,page: (products.current_page + 1)})"><i class="icon-right-open-big"></i></a>
                            <a v-else class="next page-icon"><i class="icon-right-open-big"></i></a>
                        </li>
                    </ul>
                </div>
            </div>
        </section>
    </FrontendLayout>
</template>