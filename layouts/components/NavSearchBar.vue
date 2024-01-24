<script setup>
import { useConfigStore } from '@core/stores/config';
import searchimg from '@images/icon/search.png';
import Shepherd from 'shepherd.js';
import { withQuery } from 'ufo';

defineOptions({
  // 👉 Is App Search Bar Visible
  inheritAttrs: false,
})

const configStore = useConfigStore()
const isAppSearchBarVisible = ref(false)
const searchimgicon = searchimg

// 👉 Default suggestions
const suggestionGroups = [
  {
    title: 'Popular Searches',
    content: [
      {
        icon: 'tabler-chart-donut',
        title: 'Analytics',
        url: { name: 'dashboards-analytics' },
      },
      {
        icon: 'tabler-chart-bubble',
        title: 'CRM',
        url: { name: 'dashboards-crm' },
      },
      {
        icon: 'tabler-file',
        title: 'Landing Page',
        url: { name: 'front-pages-landing-page' },
      },
      {
        icon: 'tabler-users',
        title: 'User List',
        url: { name: 'apps-user-list' },
      },
    ],
  },
  {
    title: 'Apps & Pages',
    content: [
      {
        icon: 'tabler-calendar',
        title: 'Calendar',
        url: { name: 'apps-calendar' },
      },
      {
        icon: 'tabler-shopping-cart',
        title: 'ECommerce Product',
        url: { name: 'apps-ecommerce-product-list' },
      },
      {
        icon: 'tabler-school',
        title: 'Academy',
        url: { name: 'apps-academy-dashboard' },
      },
      {
        icon: 'tabler-truck',
        title: 'Logistic Fleet',
        url: { name: 'apps-logistics-fleet' },
      },
    ],
  },
  {
    title: 'User Interface',
    content: [
      {
        icon: 'tabler-letter-a',
        title: 'Typography',
        url: { name: 'pages-typography' },
      },
      {
        icon: 'tabler-square',
        title: 'Tabs',
        url: { name: 'components-tabs' },
      },
      {
        icon: 'tabler-map',
        title: 'Tour',
        url: { name: 'extensions-tour' },
      },
      {
        icon: 'tabler-keyboard',
        title: 'Statistics',
        url: { name: 'pages-cards-card-statistics' },
      },
    ],
  },
  {
    title: 'Popular Searches',
    content: [
      {
        icon: 'tabler-list',
        title: 'Select',
        url: { name: 'forms-select' },
      },
      {
        icon: 'tabler-currency-dollar',
        title: 'Payment',
        url: { name: 'front-pages-payment' },
      },
      {
        icon: 'tabler-calendar',
        title: 'Date & Time Picker',
        url: { name: 'forms-date-time-picker' },
      },
      {
        icon: 'tabler-home',
        title: 'Property Listing Wizard',
        url: { name: 'wizard-examples-property-listing' },
      },
    ],
  },
]

// 👉 No Data suggestion
const noDataSuggestions = [
  {
    title: 'Analytics Dashboard',
    icon: 'tabler-shopping-cart',
    url: { name: 'dashboards-analytics' },
  },
  {
    title: 'Account Settings',
    icon: 'tabler-user',
    url: {
      name: 'pages-account-settings-tab',
      params: { tab: 'account' },
    },
  },
  {
    title: 'Pricing Page',
    icon: 'tabler-cash',
    url: { name: 'pages-pricing' },
  },
]

const searchQuery = ref('')
const router = useRouter()
const searchResult = ref([])

const fetchResults = async () => {
  const { data } = await useApi(withQuery('/app-bar/search', { q: searchQuery.value }))

  searchResult.value = data.value
}

watch(searchQuery, fetchResults)

const redirectToSuggestedOrSearchedPage = selected => {
  router.push(selected.url)
  isAppSearchBarVisible.value = false
  searchQuery.value = ''
}

const LazyAppBarSearch = defineAsyncComponent(() => import('@core/components/AppBarSearch.vue'))
</script>

<template>
  <div
    class="d-flex align-center cursor-pointer w-100"
    v-bind="$attrs"
    style="user-select: none;"
    @click="isAppSearchBarVisible = !isAppSearchBarVisible"
  >
    <!-- 👉 Search Trigger button -->
    <!-- close active tour while opening search bar using icon -->
    <IconBtn
      class="me-1" style="margin-left: 20px;"
      @click="Shepherd.activeTour?.cancel()"
    >
    <img
        :src="searchimgicon" 
        alt="Search"
        class="search-icon"
      />
    </IconBtn>

    <input type="text" class="meta-key" placeholder="Recherche un patient (nom, téléphone, date de naissance)">
  </div>
  
</template>

<style lang="scss" scoped>
@use "@styles/variables/_vuetify.scss";
@font-face {
  font-family: 'Geomanist-Regular';
  src: url('/fonts/Geomanist-Regular.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}
.meta-key {
  border: thin solid #133155;
  border-radius: 10px;
  block-size: 1.5625rem;
  height: 40px;
  width: 100%; /* Set width to 100% for responsiveness */
  max-width: 520px; /* Limit maximum width to 520px */
  line-height: 36px;
  padding-block: 0.125rem;
  padding-inline: 0.25rem;
  font-size: 18px;
  text-align: center;
  color: #133155;
  outline: none;
  margin-right:20px;
}
.search-icon{
  width: 24px;
  height: 24px;
}
.geomanist-regular{
  font-family: "Geomanist-Regular";
}
</style>
