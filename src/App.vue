<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Language support
const languages = {
    en: {
        welcome: 'Welcome to Boafo',
        commodity: 'Commodity',
        priceChart: 'Price Chart',
        sellableRange: 'Sellable Range',
        twoWeekForecast: '2-Week Forecast',
        trendTable: 'Monthly Trends',
        askPrice: 'Ask Price',
        cost: 'Cost',
        desiredProfit: 'Desired Profit',
        suggestion: 'Suggestion',
        hold: 'HOLD',
        sell: 'SELL',
        outOfRange: 'OUT OF RANGE',
        maize: 'Maize',
        rice: 'Rice',
        beans: 'Beans',
        currentPrice: 'Current Price',
        ceiling: 'Ceiling',
        floor: 'Floor'
    },
    twi: {
        welcome: 'Akwaaba wɔ Boafo)',
        commodity: 'Nnwuma',
        priceChart: 'Tebea Nsɛnkyerɛnne',
        sellableRange: 'Nnwuma a Wotumi Tɔn',
        twoWeekForecast: 'Nnansa 14 Nkɔmhyɛ',
        trendTable: 'Bosome Nkɔmhyɛ',
        askPrice: 'Tebea a Wɔbisa',
        cost: 'Akɔnhoma',
        desiredProfit: 'Anigye a Wopɛ',
        suggestion: 'Nsɛm',
        hold: 'SIE',
        sell: 'TƆN',
        outOfRange: 'AFI HƆ',
        maize: 'Aburo',
        rice: 'Mpataa',
        beans: 'Nkate',
        currentPrice: 'Tebea Seesei',
        ceiling: 'Soro',
        floor: 'Asase'
    },
    ga: {
        welcome: 'Woezɔ wɔ Boafo)',
        commodity: 'Nnwuma',
        priceChart: 'Tebea Nsɛnkyerɛnne',
        sellableRange: 'Nnwuma a Wotumi Tɔn',
        twoWeekForecast: 'Nnansa 14 Nkɔmhyɛ',
        trendTable: 'Bosome Nkɔmhyɛ',
        askPrice: 'Tebea a Wɔbisa',
        cost: 'Akɔnhoma',
        desiredProfit: 'Anigye a Wopɛ',
        suggestion: 'Nsɛm',
        hold: 'SIE',
        sell: 'TƆN',
        outOfRange: 'AFI HƆ',
        maize: 'Aburo',
        rice: 'Mpataa',
        beans: 'Nkate',
        currentPrice: 'Tebea Seesei',
        ceiling: 'Soro',
        floor: 'Asase'
    },
    ewe: {
        welcome: 'Woezɔ wɔ Boafo)',
        commodity: 'Nnwuma',
        priceChart: 'Tebea Nsɛnkyerɛnne',
        sellableRange: 'Nnwuma a Wotumi Tɔn',
        twoWeekForecast: 'Nnansa 14 Nkɔmhyɛ',
        trendTable: 'Bosome Nkɔmhyɛ',
        askPrice: 'Tebea a Wɔbisa',
        cost: 'Akɔnhoma',
        desiredProfit: 'Anigye a Wopɛ',
        suggestion: 'Nsɛm',
        hold: 'SIE',
        sell: 'TƆN',
        outOfRange: 'AFI HƆ',
        maize: 'Aburo',
        rice: 'Mpataa',
        beans: 'Nkate',
        currentPrice: 'Tebea Seesei',
        ceiling: 'Soro',
        floor: 'Asase'
    }
}

// Reactive state
const currentLanguage = ref('en')
const selectedCommodity = ref('maize')
const askPrice = ref('')
const cost = ref('')
const desiredProfit = ref('')
const showLanguageDropdown = ref(false)
const isDarkMode = ref(false)
const showMobileMenu = ref(false)

// Sample data
const priceData = ref({
    maize: {
        current: 450,
        ceiling: 500,
        floor: 400,
        forecast: [460, 470, 480, 475, 490, 485, 500, 495, 510, 505, 520, 515, 530, 525]
    },
    rice: {
        current: 380,
        ceiling: 420,
        floor: 350,
        forecast: [385, 390, 395, 400, 405, 410, 415, 420, 425, 430, 435, 440, 445, 450]
    },
    beans: {
        current: 320,
        ceiling: 360,
        floor: 280,
        forecast: [325, 330, 335, 340, 345, 350, 355, 360, 365, 370, 375, 380, 385, 390]
    }
})

const monthlyTrends = ref({
    maize: [
        { month: 'Jan', price: 420, trend: 'up' },
        { month: 'Feb', price: 435, trend: 'up' },
        { month: 'Mar', price: 450, trend: 'up' },
        { month: 'Apr', price: 440, trend: 'down' },
        { month: 'May', price: 460, trend: 'up' },
        { month: 'Jun', price: 470, trend: 'up' }
    ],
    rice: [
        { month: 'Jan', price: 360, trend: 'up' },
        { month: 'Feb', price: 370, trend: 'up' },
        { month: 'Mar', price: 380, trend: 'up' },
        { month: 'Apr', price: 375, trend: 'down' },
        { month: 'May', price: 385, trend: 'up' },
        { month: 'Jun', price: 390, trend: 'up' }
    ],
    beans: [
        { month: 'Jan', price: 300, trend: 'up' },
        { month: 'Feb', price: 310, trend: 'up' },
        { month: 'Mar', price: 320, trend: 'up' },
        { month: 'Apr', price: 315, trend: 'down' },
        { month: 'May', price: 325, trend: 'up' },
        { month: 'Jun', price: 330, trend: 'up' }
    ]
})

// Computed properties
const t = computed(() => languages[currentLanguage.value])
const currentPriceData = computed(() => priceData.value[selectedCommodity.value])
const currentTrends = computed(() => monthlyTrends.value[selectedCommodity.value])

const suggestion = computed(() => {
    if (!askPrice.value || !cost.value || !desiredProfit.value) return null

    const ask = parseFloat(askPrice.value)
    const costPrice = parseFloat(cost.value)
    const profit = parseFloat(desiredProfit.value)
    const current = currentPriceData.value.current
    const ceiling = currentPriceData.value.ceiling
    const floor = currentPriceData.value.floor

    if (ask < floor || ask > ceiling) {
        return { type: 'out', message: t.value.outOfRange }
    }

    const potentialProfit = ask - costPrice
    if (potentialProfit >= profit) {
        return { type: 'sell', message: t.value.sell }
    } else {
        return { type: 'hold', message: t.value.hold }
    }
})

const isInRange = computed(() => {
    const current = currentPriceData.value.current
    const ceiling = currentPriceData.value.ceiling
    const floor = currentPriceData.value.floor
    return current >= floor && current <= ceiling
})

// Methods
const switchLanguage = (lang) => {
    currentLanguage.value = lang
}

const switchCommodity = (commodity) => {
    selectedCommodity.value = commodity
}

const toggleLanguageDropdown = () => {
    showLanguageDropdown.value = !showLanguageDropdown.value
}

const selectLanguage = (lang) => {
    currentLanguage.value = lang
    showLanguageDropdown.value = false
}

const getLanguageName = (code) => {
    const languageNames = {
        en: 'English',
        twi: 'Twi',
        ga: 'Ga',
        ewe: 'Ewe'
    }
    return languageNames[code] || code.toUpperCase()
}

const getTrendIcon = (trend) => {
    switch (trend) {
        case 'up': return '↗'
        case 'down': return '↘'
        default: return '→'
    }
}

const getTrendClass = (trend) => {
    switch (trend) {
        case 'up': return 'trend-up'
        case 'down': return 'trend-down'
        default: return 'trend-neutral'
    }
}

// Click outside handler for dropdown
const handleClickOutside = (event) => {
    const dropdown = event.target.closest('.language-dropdown')
    const mobileMenu = event.target.closest('.mobile-menu')
    if (!dropdown) {
        showLanguageDropdown.value = false
    }
    if (!mobileMenu) {
        showMobileMenu.value = false
    }
}

// Dark mode toggle
const toggleDarkMode = () => {
    isDarkMode.value = !isDarkMode.value
    document.documentElement.classList.toggle('dark', isDarkMode.value)
    localStorage.setItem('darkMode', isDarkMode.value.toString())
}

// Mobile menu toggle
const toggleMobileMenu = () => {
    showMobileMenu.value = !showMobileMenu.value
    console.log('Mobile menu toggled:', showMobileMenu.value)
}

onMounted(() => {
    document.addEventListener('click', handleClickOutside)
    // Initialize dark mode from localStorage
    const savedDarkMode = localStorage.getItem('darkMode')
    if (savedDarkMode === 'true') {
        isDarkMode.value = true
        document.documentElement.classList.add('dark')
    }
})

onUnmounted(() => {
    document.removeEventListener('click', handleClickOutside)
})
</script>

<template>
    <div
        class="min-h-screen bg-gradient-to-br from-amber-50 via-orange-50 to-yellow-50 dark:from-gray-900 dark:via-gray-800 dark:to-gray-900 transition-colors duration-300">
        <!-- Header -->
        <header class="bg-white/80 dark:bg-gray-900/80 backdrop-blur-sm shadow-lg border-b border-amber-200 dark:border-gray-700 sticky top-0 z-50">
            <nav aria-label="Global" class="flex items-center justify-between p-4 lg:px-8 max-w-7xl mx-auto">
                <div class="flex lg:flex-1">
                    <a href="#" class="-m-1.5 p-1.5 group">
                        <span class="sr-only">Boafo</span>
                        <div class="flex items-center space-x-3 group-hover:scale-105 transition-transform duration-200">
                            <div class="w-10 h-10 bg-gradient-to-br from-green-600 via-green-700 to-emerald-800 rounded-xl flex items-center justify-center shadow-lg ring-1 ring-green-500/20">
                                <span class="text-white font-bold text-xl">B</span>
                            </div>
                            <div class="brand-logo text-2xl dark:text-white font-bold">Boafo</div>
                        </div>
                    </a>
                </div>
                <div class="flex lg:hidden items-center space-x-2">
                    <!-- Dark Mode Toggle -->
                    <button @click="toggleDarkMode" class="p-2 rounded-lg text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors">
                        <svg v-if="!isDarkMode" class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path>
                        </svg>
                        <svg v-else class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path>
                        </svg>
                    </button>
                    <!-- Language Dropdown -->
                    <div class="relative language-dropdown">
                        <button @click="toggleLanguageDropdown" class="flex items-center space-x-1 px-2 py-1 text-gray-700 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white font-medium transition-colors rounded hover:bg-gray-50 dark:hover:bg-gray-800">
                            <span class="text-base">{{ getLanguageName(currentLanguage) }}</span>
                            <svg class="w-3 h-3 transition-transform duration-200" :class="{ 'rotate-180': showLanguageDropdown }" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
                            </svg>
                        </button>
                        <!-- Dropdown Menu -->
                        <div v-if="showLanguageDropdown" class="absolute top-full right-0 mt-1 w-40 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-lg shadow-lg z-50 overflow-hidden">
                            <template v-for="(lang, code, index) in languages" :key="code">
                                <button @click="selectLanguage(code)" :class="['w-full flex items-center justify-between px-3 py-2 text-left hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors duration-200', currentLanguage === code ? 'bg-gray-50 dark:bg-gray-700 text-gray-900 dark:text-white font-medium' : 'text-gray-700 dark:text-gray-300']">
                                    <span class="text-sm">{{ getLanguageName(code) }}</span>
                                    <svg v-if="currentLanguage === code" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-4 text-green-600 dark:text-green-400">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
                                    </svg>
                                </button>
                                <div v-if="index < Object.keys(languages).length - 1" class="border-t border-gray-100 dark:border-gray-700"></div>
                            </template>
                        </div>
                    </div>
                    <!-- Mobile Menu Button -->
                    <button type="button" command="show-modal" commandfor="mobile-menu" class="-m-2.5 inline-flex items-center justify-center rounded-md p-2.5 text-gray-700 dark:text-gray-400">
                        <span class="sr-only">Open main menu</span>
                        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" data-slot="icon" aria-hidden="true" class="size-6">
                            <path d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" stroke-linecap="round" stroke-linejoin="round" />
                        </svg>
                    </button>
                </div>
                <div class="hidden lg:flex lg:gap-x-12">
                    <a href="#prices" class="text-base font-semibold text-gray-700 dark:text-gray-300 hover:text-green-600 dark:hover:text-green-400 transition-colors duration-200 relative group">
                        Prices
                        <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-green-600 group-hover:w-full transition-all duration-200"></span>
                    </a>
                    <a href="#calculator" class="text-base font-semibold text-gray-700 dark:text-gray-300 hover:text-green-600 dark:hover:text-green-400 transition-colors duration-200 relative group">
                        Calculator
                        <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-green-600 group-hover:w-full transition-all duration-200"></span>
                    </a>
                    <a href="#forecast" class="text-base font-semibold text-gray-700 dark:text-gray-300 hover:text-green-600 dark:hover:text-green-400 transition-colors duration-200 relative group">
                        Forecast
                        <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-green-600 group-hover:w-full transition-all duration-200"></span>
                    </a>
                    <a href="#help" class="text-base font-semibold text-gray-700 dark:text-gray-300 hover:text-green-600 dark:hover:text-green-400 transition-colors duration-200 relative group">
                        Help
                        <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-green-600 group-hover:w-full transition-all duration-200"></span>
                    </a>
                </div>
                <div class="hidden lg:flex lg:flex-1 lg:justify-end lg:items-center lg:gap-x-3">
                    <!-- Dark Mode Toggle -->
                    <button @click="toggleDarkMode" class="p-2.5 rounded-xl text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 transition-all duration-200 hover:scale-105 group">
                        <svg v-if="!isDarkMode" class="w-5 h-5 group-hover:rotate-12 transition-transform duration-200" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path>
                        </svg>
                        <svg v-else class="w-5 h-5 group-hover:rotate-12 transition-transform duration-200" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path>
                        </svg>
                    </button>
                    <!-- Language Dropdown -->
                    <div class="relative language-dropdown">
                        <button @click="toggleLanguageDropdown" class="flex items-center space-x-2 px-3 py-2 text-gray-700 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white font-medium transition-all duration-200 rounded-xl hover:bg-gray-100 dark:hover:bg-gray-800 group">
                            <span class="text-base font-semibold">{{ getLanguageName(currentLanguage) }}</span>
                            <svg class="w-4 h-4 transition-all duration-200 group-hover:scale-110" :class="{ 'rotate-180': showLanguageDropdown }" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
                            </svg>
                        </button>
                        <!-- Dropdown Menu -->
                        <div v-if="showLanguageDropdown" class="absolute top-full right-0 mt-2 w-44 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-xl shadow-xl z-50 overflow-hidden backdrop-blur-sm">
                            <template v-for="(lang, code, index) in languages" :key="code">
                                <button @click="selectLanguage(code)" :class="['w-full flex items-center justify-between px-4 py-3 text-left hover:bg-gray-50 dark:hover:bg-gray-700 transition-all duration-200 group', currentLanguage === code ? 'bg-green-50 dark:bg-green-900/30 text-green-700 dark:text-green-300 font-semibold' : 'text-gray-700 dark:text-gray-300']">
                                    <span class="text-sm">{{ getLanguageName(code) }}</span>
                                    <svg v-if="currentLanguage === code" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="size-4 text-green-600 dark:text-green-400 group-hover:scale-110 transition-transform duration-200">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
                                    </svg>
                                </button>
                                <div v-if="index < Object.keys(languages).length - 1" class="border-t border-gray-100 dark:border-gray-700"></div>
                            </template>
                        </div>
                    </div>
                </div>
            </nav>
            <el-dialog>
                <dialog id="mobile-menu" class="backdrop:bg-transparent lg:hidden">
                    <div tabindex="0" class="fixed inset-0 focus:outline-none">
                        <el-dialog-panel
                            class="fixed inset-y-0 right-0 z-50 w-full overflow-y-auto bg-white p-6 sm:max-w-sm sm:ring-1 sm:ring-gray-900/10 dark:bg-gray-900 dark:sm:ring-gray-100/10">
                            <div class="flex items-center justify-between">
                                <a href="#" class="-m-1.5 p-1.5">
                                    <span class="sr-only">Boafo</span>
                                    <div class="flex items-center space-x-2">
                                        <div
                                            class="w-8 h-8 bg-gradient-to-br from-green-600 to-green-800 rounded-xl flex items-center justify-center shadow-lg">
                                            <span class="text-white font-bold text-lg">B</span>
                                        </div>
                                        <div class="brand-logo text-xl dark:text-white">Boafo</div>
                                    </div>
                                </a>
                                <button type="button" command="close" commandfor="mobile-menu"
                                    class="-m-2.5 rounded-md p-2.5 text-gray-700 dark:text-gray-400">
                                    <span class="sr-only">Close menu</span>
                                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"
                                        data-slot="icon" aria-hidden="true" class="size-6">
                                        <path d="M6 18 18 6M6 6l12 12" stroke-linecap="round" stroke-linejoin="round" />
                                    </svg>
                                </button>
                            </div>
                            <div class="mt-6 flow-root">
                                <div class="-my-6 divide-y divide-gray-500/10 dark:divide-white/10">
                                    <div class="space-y-2 py-6">
                                        <a href="#prices"
                                            class="-mx-3 block rounded-lg px-3 py-2 text-lg font-semibold text-gray-900 hover:bg-gray-50 dark:text-white dark:hover:bg-white/5">Prices</a>
                                        <a href="#calculator"
                                            class="-mx-3 block rounded-lg px-3 py-2 text-lg font-semibold text-gray-900 hover:bg-gray-50 dark:text-white dark:hover:bg-white/5">Calculator</a>
                                        <a href="#forecast"
                                            class="-mx-3 block rounded-lg px-3 py-2 text-lg font-semibold text-gray-900 hover:bg-gray-50 dark:text-white dark:hover:bg-white/5">Forecast</a>
                                        <a href="#help"
                                            class="-mx-3 block rounded-lg px-3 py-2 text-lg font-semibold text-gray-900 hover:bg-gray-50 dark:text-white dark:hover:bg-white/5">Help</a>
                                    </div>

                                </div>
                            </div>
                        </el-dialog-panel>
                    </div>
                </dialog>
            </el-dialog>
        </header>





        <!-- Enhanced Hero Section -->
        <section class="relative overflow-hidden min-h-screen flex items-center">
            <!-- Background Elements -->
            <div class="absolute inset-0 bg-gradient-to-br from-green-50 via-amber-50 to-orange-50"></div>

            <div class="relative max-w-7xl mx-auto px-4 sm:px-6 py-12">
                <div class="grid lg:grid-cols-2 gap-16 items-center">
                    <!-- Left Column - Enhanced Content -->
                    <div class="text-center lg:text-left">
                        <!-- Enhanced Badge -->
                        <div
                            class="inline-flex items-center px-6 py-3 bg-gradient-to-r from-green-100 to-emerald-100 text-green-800 rounded-full text-sm font-semibold mb-8 shadow-lg border border-green-200">
                            <div class="w-3 h-3 bg-green-500 rounded-full mr-3"></div>
                            <span class="mr-2">🌾</span>
                            Smart Pricing for Farmers & Traders
                            <span class="ml-2">💰</span>
                        </div>

                        <!-- Enhanced Headline -->
                        <h1 class="text-5xl md:text-6xl lg:text-7xl font-bold text-gray-900 mb-8 leading-tight">
                            <span class="block">Smart Pricing</span>
                            <span
                                class="bg-gradient-to-r from-green-600 via-emerald-600 to-green-800 bg-clip-text text-transparent">
                                Decisions
                            </span>
                        </h1>

                        <!-- Enhanced Description -->
                        <p class="text-xl md:text-2xl text-gray-600 mb-10 leading-relaxed max-w-2xl">
                            Help farmers, traders, and small businesses make <span
                                class="font-semibold text-green-600">smarter
                                commodity pricing decisions</span>
                            with real-time market data, profit calculations, and trend analysis. Available in
                            <span class="font-semibold text-amber-600">English, Twi, Ga, and Ewe</span> languages.
                        </p>

                        <!-- Enhanced CTA Buttons -->
                        <div class="flex flex-col sm:flex-row gap-6 justify-center lg:justify-start mb-12">
                            <button
                                class="group bg-gradient-to-r from-green-600 via-green-700 to-emerald-700 text-white px-10 py-5 rounded-2xl font-bold text-lg hover:shadow-2xl flex items-center justify-center relative overflow-hidden">
                                <svg class="w-6 h-6 mr-3 relative z-10" fill="none" stroke="currentColor"
                                    viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z">
                                    </path>
                                </svg>
                                <span class="relative z-10">Check Prices Now</span>
                            </button>
                            <button
                                class="group bg-white dark:bg-gray-800 text-gray-700 dark:text-gray-200 px-10 py-5 rounded-2xl font-bold text-lg border-2 border-gray-200 dark:border-gray-600 hover:border-green-300 dark:hover:border-green-400 hover:shadow-xl flex items-center justify-center transition-all duration-300">
                                <span class="w-6 h-6 mr-3 flex items-center justify-center text-lg font-bold">₵</span>
                                Calculate Profit
                            </button>
                        </div>

                        <!-- Enhanced Stats -->
                        <div class="grid grid-cols-3 gap-8 pt-8 border-t border-gray-200">
                            <div class="text-center group">
                                <div class="text-4xl font-bold text-green-600">10K+</div>
                                <div class="text-sm text-gray-600 font-medium">Farmers</div>
                                <div class="text-xs text-gray-500 mt-1">Using our platform</div>
                            </div>
                            <div class="text-center group">
                                <div class="text-4xl font-bold text-green-600">50+</div>
                                <div class="text-sm text-gray-600 font-medium">Commodities</div>
                                <div class="text-xs text-gray-500 mt-1">Price tracking</div>
                            </div>
                            <div class="text-center group">
                                <div class="text-4xl font-bold text-green-600">100%</div>
                                <div class="text-sm text-gray-600 font-medium">Free</div>
                                <div class="text-xs text-gray-500 mt-1">For all users</div>
                            </div>
                        </div>
                    </div>

                    <!-- Right Column - Redesigned Visual -->
                    <div class="relative">
                        <!-- Main Feature Cards Grid -->
                        <div class="grid grid-cols-2 gap-6">
                            <!-- Price Card -->
                            <div
                                class="bg-gradient-to-br from-green-500 to-emerald-600 rounded-2xl p-6 text-white shadow-xl">
                                <div class="flex items-center justify-between mb-4">
                                    <div class="w-8 h-8 bg-white/20 rounded-lg flex items-center justify-center">
                                        <span
                                            class="w-5 h-5 flex items-center justify-center text-sm font-bold">₵</span>
                                    </div>
                                    <span class="text-xs bg-white/20 px-2 py-1 rounded-full">Live</span>
                                </div>
                                <div class="text-2xl font-bold mb-1">₵450</div>
                                <div class="text-sm opacity-90">Current Price</div>
                                <div class="flex items-center mt-2">
                                    <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
                                    </svg>
                                    <span class="text-xs">+2.5%</span>
                                </div>
                            </div>

                            <!-- Range Card -->
                            <div
                                class="bg-gradient-to-br from-blue-500 to-indigo-600 rounded-2xl p-6 text-white shadow-xl">
                                <div class="flex items-center justify-between mb-4">
                                    <div class="w-8 h-8 bg-white/20 rounded-lg flex items-center justify-center">
                                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z">
                                            </path>
                                        </svg>
                                    </div>
                                    <span class="text-xs bg-white/20 px-2 py-1 rounded-full">Range</span>
                                </div>
                                <div class="text-lg font-bold mb-1">₵400 - ₵500</div>
                                <div class="text-sm opacity-90">Sellable Range</div>
                                <div class="mt-3 h-2 bg-white/30 rounded-full overflow-hidden">
                                    <div class="h-full bg-white rounded-full w-3/4"></div>
                                </div>
                            </div>

                            <!-- Commodity Card -->
                            <div
                                class="bg-gradient-to-br from-amber-500 to-orange-600 rounded-2xl p-6 text-white shadow-xl">
                                <div class="flex items-center justify-between mb-4">
                                    <div class="w-8 h-8 bg-white/20 rounded-lg flex items-center justify-center">
                                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4">
                                            </path>
                                        </svg>
                                    </div>
                                    <span class="text-xs bg-white/20 px-2 py-1 rounded-full">Active</span>
                                </div>
                                <div class="text-xl font-bold mb-1">Maize</div>
                                <div class="text-sm opacity-90">Selected Commodity</div>
                                <div class="flex items-center mt-2">
                                    <div class="w-2 h-2 bg-white rounded-full mr-2"></div>
                                    <span class="text-xs">Trading</span>
                                </div>
                            </div>

                            <!-- Suggestion Card -->
                            <div
                                class="bg-gradient-to-br from-purple-500 to-pink-600 rounded-2xl p-6 text-white shadow-xl">
                                <div class="flex items-center justify-between mb-4">
                                    <div class="w-8 h-8 bg-white/20 rounded-lg flex items-center justify-center">
                                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z">
                                            </path>
                                        </svg>
                                    </div>
                                    <span class="text-xs bg-white/20 px-2 py-1 rounded-full">AI</span>
                                </div>
                                <div class="text-xl font-bold mb-1">HOLD</div>
                                <div class="text-sm opacity-90">Smart Suggestion</div>
                                <div class="flex items-center mt-2">
                                    <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            d="M5 13l4 4L19 7"></path>
                                    </svg>
                                    <span class="text-xs">Recommended</span>
                                </div>
                            </div>
                        </div>

                        <!-- Central Dashboard Preview -->
                        <div
                            class="mt-8 bg-white/95 dark:bg-gray-800/90 backdrop-blur-sm rounded-3xl p-6 shadow-2xl border border-gray-100 dark:border-gray-700">
                            <div class="flex items-center justify-between mb-6">
                                <div class="flex items-center space-x-3">
                                    <div>
                                        <div class="text-xs text-gray-500 dark:text-gray-400">Trading Dashboard</div>
                                    </div>
                                </div>
                                <div class="flex space-x-1">
                                    <div class="w-2 h-2 bg-red-400 rounded-full"></div>
                                    <div class="w-2 h-2 bg-yellow-400 rounded-full"></div>
                                    <div class="w-2 h-2 bg-green-400 rounded-full"></div>
                                </div>
                            </div>

                            <!-- Mini Chart -->
                            <div
                                class="bg-gradient-to-r from-gray-50 to-gray-100 dark:from-gray-800 dark:to-gray-700 rounded-xl p-4 mb-4">
                                <div class="flex items-center justify-between mb-2">
                                    <span class="text-sm font-medium text-gray-600 dark:text-gray-300">Price
                                        Trend</span>
                                    <span class="text-xs text-green-600 dark:text-green-400 font-semibold">↗
                                        +5.2%</span>
                                </div>
                                <div class="h-16 bg-white dark:bg-gray-900 rounded-lg p-2 flex items-end space-x-1">
                                    <div class="w-2 bg-green-400 rounded-t" style="height: 40%"></div>
                                    <div class="w-2 bg-green-400 rounded-t" style="height: 55%"></div>
                                    <div class="w-2 bg-green-400 rounded-t" style="height: 70%"></div>
                                    <div class="w-2 bg-green-400 rounded-t" style="height: 60%"></div>
                                    <div class="w-2 bg-green-400 rounded-t" style="height: 85%"></div>
                                    <div class="w-2 bg-green-400 rounded-t" style="height: 90%"></div>
                                    <div class="w-2 bg-green-400 rounded-t" style="height: 100%"></div>
                                </div>
                            </div>

                            <!-- Quick Actions -->
                            <div class="grid grid-cols-3 gap-3">
                                <button
                                    class="bg-green-50 dark:bg-green-900/30 text-green-700 dark:text-green-300 py-2 px-3 rounded-lg text-sm font-medium hover:bg-green-100 dark:hover:bg-green-900/40 transition-colors">
                                    Price Check
                                </button>
                                <button
                                    class="bg-blue-50 dark:bg-blue-900/30 text-blue-700 dark:text-blue-300 py-2 px-3 rounded-lg text-sm font-medium hover:bg-blue-100 dark:hover:bg-blue-900/40 transition-colors">
                                    Calculate
                                </button>
                                <button
                                    class="bg-purple-50 dark:bg-purple-900/30 text-purple-700 dark:text-purple-300 py-2 px-3 rounded-lg text-sm font-medium hover:bg-purple-100 dark:hover:bg-purple-900/40 transition-colors">
                                    Forecast
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <main class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 py-12 space-y-16">
            <!-- Commodity Price Tracker Section -->
            <section
                class="bg-white dark:bg-gray-900 rounded-3xl p-8 shadow-lg border border-gray-100 dark:border-gray-800">
                <!-- Simplified Header -->
                <div class="text-center mb-12">
                    <h2 class="text-4xl font-bold mb-4 text-gray-900 dark:text-gray-100">Live Price Tracker</h2>
                    <p class="text-gray-600 dark:text-gray-300 text-lg max-w-2xl mx-auto">
                        Real-time commodity prices updated every hour. Make informed decisions with current market data.
                    </p>
                </div>

                <!-- Clean Search and Filter -->
                <div class="flex flex-col sm:flex-row gap-4 justify-center mb-8">
                    <div class="relative max-w-md mx-auto sm:mx-0">
                        <input type="text" placeholder="Search commodities..."
                            class="w-full pl-10 pr-4 py-3 border-0 outline outline-1 outline-gray-300 dark:outline-gray-700 rounded-xl focus:outline-2 focus:outline-green-500 bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100 placeholder-gray-400 dark:placeholder-gray-500">
                        <svg class="absolute left-3 top-3.5 w-5 h-5 text-gray-400 dark:text-gray-500" fill="none"
                            stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
                        </svg>
                    </div>
                    <div class="relative">
                        <select
                            class="appearance-none px-6 py-3 pr-10 border-0 outline outline-1 outline-gray-300 dark:outline-gray-700 rounded-xl focus:outline-2 focus:outline-green-500 bg-white dark:bg-gray-800 text-gray-700 dark:text-gray-100 font-medium cursor-pointer hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors">
                            <option value="">All Categories</option>
                            <option value="cereals">Cereals</option>
                            <option value="legumes">Legumes</option>
                            <option value="root-crops">Root Crops</option>
                            <option value="export-crops">Export Crops</option>
                        </select>
                        <div class="absolute inset-y-0 right-0 flex items-center pr-3 pointer-events-none">
                            <svg class="w-5 h-5 text-gray-400 dark:text-gray-500" fill="none" stroke="currentColor"
                                viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M19 9l-7 7-7-7"></path>
                            </svg>
                        </div>
                    </div>
                </div>

                <!-- Simplified Market Status -->
                <div
                    class="bg-green-50 dark:bg-green-900/20 rounded-2xl p-6 mb-8 border border-green-100 dark:border-green-800">
                    <div class="flex flex-col sm:flex-row justify-between items-center gap-4">
                        <div class="flex items-center space-x-3">
                            <div class="w-3 h-3 bg-green-500 rounded-full"></div>
                            <span class="font-semibold text-green-800 dark:text-green-300">Market Open</span>
                            <span class="text-sm text-green-600 dark:text-green-400">Updated 2 min ago</span>
                        </div>
                        <div class="flex items-center space-x-8 text-sm">
                            <div class="text-center">
                                <div class="font-bold text-green-600 dark:text-green-400 text-lg">12</div>
                                <div class="text-gray-600 dark:text-gray-300">Rising</div>
                            </div>
                            <div class="text-center">
                                <div class="font-bold text-red-600 dark:text-red-400 text-lg">3</div>
                                <div class="text-gray-600 dark:text-gray-300">Falling</div>
                            </div>
                            <div class="text-center">
                                <div class="font-bold text-gray-600 dark:text-gray-300 text-lg">5</div>
                                <div class="text-gray-600 dark:text-gray-300">Stable</div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Clean Commodity Cards -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Maize Card -->
                    <div class="group cursor-pointer">
                        <div
                            class="bg-gray-50 dark:bg-gray-800 rounded-2xl p-6 hover:bg-white dark:hover:bg-gray-700 hover:shadow-lg transition-all duration-300 border border-gray-100 dark:border-gray-700">
                            <div class="flex items-center justify-between mb-6">
                                <div class="flex items-center space-x-4">
                                    <div
                                        class="w-12 h-12 bg-gradient-to-br from-yellow-400 to-yellow-500 rounded-xl flex items-center justify-center">
                                        <svg class="w-6 h-6 text-white" fill="currentColor" viewBox="0 0 24 24">
                                            <path
                                                d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                        </svg>
                                    </div>
                                    <div>
                                        <h3 class="font-bold text-gray-900 dark:text-gray-100 text-lg">Maize</h3>
                                        <p class="text-sm text-gray-500 dark:text-gray-400">Cereal • Per 100kg</p>
                                    </div>
                                </div>
                                <div class="text-right">
                                    <div class="text-2xl font-bold text-gray-900 dark:text-gray-100">₵450</div>
                                    <div
                                        class="flex items-center text-green-600 dark:text-green-400 text-sm font-medium">
                                        <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
                                        </svg>
                                        +2.5%
                                    </div>
                                </div>
                            </div>
                            <div class="space-y-2">
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h High:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵465</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h Low:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵435</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">Volume:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">2,450 bags</span>
                                </div>
                            </div>
                            <div class="mt-4 pt-4 border-t border-gray-200 dark:border-gray-700">
                                <div class="flex items-center justify-between">
                                    <span class="text-sm text-gray-500 dark:text-gray-400">Trend</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-2 h-2 bg-green-500 rounded-full"></div>
                                        <span
                                            class="text-sm font-medium text-green-600 dark:text-green-400">Bullish</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Rice Card -->
                    <div class="group cursor-pointer">
                        <div
                            class="bg-gray-50 dark:bg-gray-800 rounded-2xl p-6 hover:bg-white dark:hover:bg-gray-700 hover:shadow-lg transition-all duration-300 border border-gray-100 dark:border-gray-700">
                            <div class="flex items-center justify-between mb-6">
                                <div class="flex items-center space-x-4">
                                    <div
                                        class="w-12 h-12 bg-gradient-to-br from-amber-400 to-amber-500 rounded-xl flex items-center justify-center">
                                        <svg class="w-6 h-6 text-white" fill="currentColor" viewBox="0 0 24 24">
                                            <path
                                                d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                        </svg>
                                    </div>
                                    <div>
                                        <h3 class="font-bold text-gray-900 dark:text-gray-100 text-lg">Rice</h3>
                                        <p class="text-sm text-gray-500 dark:text-gray-400">Grain • Per 50kg</p>
                                    </div>
                                </div>
                                <div class="text-right">
                                    <div class="text-2xl font-bold text-gray-900 dark:text-gray-100">₵380</div>
                                    <div
                                        class="flex items-center text-green-600 dark:text-green-400 text-sm font-medium">
                                        <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
                                        </svg>
                                        +1.8%
                                    </div>
                                </div>
                            </div>
                            <div class="space-y-2">
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h High:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵390</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h Low:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵375</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">Volume:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">1,820 bags</span>
                                </div>
                            </div>
                            <div class="mt-4 pt-4 border-t border-gray-200 dark:border-gray-700">
                                <div class="flex items-center justify-between">
                                    <span class="text-sm text-gray-500 dark:text-gray-400">Trend</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-2 h-2 bg-green-500 rounded-full"></div>
                                        <span
                                            class="text-sm font-medium text-green-600 dark:text-green-400">Bullish</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Beans Card -->
                    <div class="group cursor-pointer">
                        <div
                            class="bg-gray-50 dark:bg-gray-800 rounded-2xl p-6 hover:bg-white dark:hover:bg-gray-700 hover:shadow-lg transition-all duration-300 border border-gray-100 dark:border-gray-700">
                            <div class="flex items-center justify-between mb-6">
                                <div class="flex items-center space-x-4">
                                    <div
                                        class="w-12 h-12 bg-gradient-to-br from-green-400 to-green-500 rounded-xl flex items-center justify-center">
                                        <svg class="w-6 h-6 text-white" fill="currentColor" viewBox="0 0 24 24">
                                            <path
                                                d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                        </svg>
                                    </div>
                                    <div>
                                        <h3 class="font-bold text-gray-900 dark:text-gray-100 text-lg">Beans</h3>
                                        <p class="text-sm text-gray-500 dark:text-gray-400">Legume • Per 100kg</p>
                                    </div>
                                </div>
                                <div class="text-right">
                                    <div class="text-2xl font-bold text-gray-900 dark:text-gray-100">₵320</div>
                                    <div
                                        class="flex items-center text-green-600 dark:text-green-400 text-sm font-medium">
                                        <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
                                        </svg>
                                        +3.2%
                                    </div>
                                </div>
                            </div>
                            <div class="space-y-2">
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h High:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵335</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h Low:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵310</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">Volume:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">1,150 bags</span>
                                </div>
                            </div>
                            <div class="mt-4 pt-4 border-t border-gray-200 dark:border-gray-700">
                                <div class="flex items-center justify-between">
                                    <span class="text-sm text-gray-500 dark:text-gray-400">Trend</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-2 h-2 bg-green-500 rounded-full"></div>
                                        <span
                                            class="text-sm font-medium text-green-600 dark:text-green-400">Bullish</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Cassava Card -->
                    <div class="group cursor-pointer">
                        <div
                            class="bg-gray-50 dark:bg-gray-800 rounded-2xl p-6 hover:bg-white dark:hover:bg-gray-700 hover:shadow-lg transition-all duration-300 border border-gray-100 dark:border-gray-700">
                            <div class="flex items-center justify-between mb-6">
                                <div class="flex items-center space-x-4">
                                    <div
                                        class="w-12 h-12 bg-gradient-to-br from-orange-400 to-orange-500 rounded-xl flex items-center justify-center">
                                        <svg class="w-6 h-6 text-white" fill="currentColor" viewBox="0 0 24 24">
                                            <path
                                                d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                        </svg>
                                    </div>
                                    <div>
                                        <h3 class="font-bold text-gray-900 dark:text-gray-100 text-lg">Cassava</h3>
                                        <p class="text-sm text-gray-500 dark:text-gray-400">Root Crop • Per 100kg</p>
                                    </div>
                                </div>
                                <div class="text-right">
                                    <div class="text-2xl font-bold text-gray-900 dark:text-gray-100">₵280</div>
                                    <div
                                        class="flex items-center text-green-600 dark:text-green-400 text-sm font-medium">
                                        <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
                                        </svg>
                                        +1.5%
                                    </div>
                                </div>
                            </div>
                            <div class="space-y-2">
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h High:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵285</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h Low:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵275</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">Volume:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">3,200 bags</span>
                                </div>
                            </div>
                            <div class="mt-4 pt-4 border-t border-gray-200 dark:border-gray-700">
                                <div class="flex items-center justify-between">
                                    <span class="text-sm text-gray-500 dark:text-gray-400">Trend</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-2 h-2 bg-gray-400 rounded-full"></div>
                                        <span class="text-sm font-medium text-gray-600 dark:text-gray-400">Stable</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Yam Card -->
                    <div class="group cursor-pointer">
                        <div
                            class="bg-gray-50 dark:bg-gray-800 rounded-2xl p-6 hover:bg-white dark:hover:bg-gray-700 hover:shadow-lg transition-all duration-300 border border-gray-100 dark:border-gray-700">
                            <div class="flex items-center justify-between mb-6">
                                <div class="flex items-center space-x-4">
                                    <div
                                        class="w-12 h-12 bg-gradient-to-br from-purple-400 to-purple-500 rounded-xl flex items-center justify-center">
                                        <svg class="w-6 h-6 text-white" fill="currentColor" viewBox="0 0 24 24">
                                            <path
                                                d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                        </svg>
                                    </div>
                                    <div>
                                        <h3 class="font-bold text-gray-900 dark:text-gray-100 text-lg">Yam</h3>
                                        <p class="text-sm text-gray-500 dark:text-gray-400">Tuber • Per 100kg</p>
                                    </div>
                                </div>
                                <div class="text-right">
                                    <div class="text-2xl font-bold text-gray-900 dark:text-gray-100">₵520</div>
                                    <div
                                        class="flex items-center text-green-600 dark:text-green-400 text-sm font-medium">
                                        <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
                                        </svg>
                                        +4.1%
                                    </div>
                                </div>
                            </div>
                            <div class="space-y-2">
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h High:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵535</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h Low:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵505</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">Volume:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">890 bags</span>
                                </div>
                            </div>
                            <div class="mt-4 pt-4 border-t border-gray-200 dark:border-gray-700">
                                <div class="flex items-center justify-between">
                                    <span class="text-sm text-gray-500 dark:text-gray-400">Trend</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-2 h-2 bg-green-500 rounded-full"></div>
                                        <span
                                            class="text-sm font-medium text-green-600 dark:text-green-400">Bullish</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Plantain Card -->
                    <div class="group cursor-pointer">
                        <div
                            class="bg-gray-50 dark:bg-gray-800 rounded-2xl p-6 hover:bg-white dark:hover:bg-gray-700 hover:shadow-lg transition-all duration-300 border border-gray-100 dark:border-gray-700">
                            <div class="flex items-center justify-between mb-6">
                                <div class="flex items-center space-x-4">
                                    <div
                                        class="w-12 h-12 bg-gradient-to-br from-lime-400 to-lime-500 rounded-xl flex items-center justify-center">
                                        <svg class="w-6 h-6 text-white" fill="currentColor" viewBox="0 0 24 24">
                                            <path
                                                d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                        </svg>
                                    </div>
                                    <div>
                                        <h3 class="font-bold text-gray-900 dark:text-gray-100 text-lg">Plantain</h3>
                                        <p class="text-sm text-gray-500 dark:text-gray-400">Fruit • Per bunch</p>
                                    </div>
                                </div>
                                <div class="text-right">
                                    <div class="text-2xl font-bold text-gray-900 dark:text-gray-100">₵180</div>
                                    <div class="flex items-center text-red-600 dark:text-red-400 text-sm font-medium">
                                        <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M13 17h8m0 0V9m0 8l-8-8-4 4-6-6"></path>
                                        </svg>
                                        -2.8%
                                    </div>
                                </div>
                            </div>
                            <div class="space-y-2">
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h High:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵195</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">24h Low:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">₵175</span>
                                </div>
                                <div class="flex justify-between text-sm">
                                    <span class="text-gray-500 dark:text-gray-400">Volume:</span>
                                    <span class="font-medium text-gray-900 dark:text-gray-100">1,420 bunches</span>
                                </div>
                            </div>
                            <div class="mt-4 pt-4 border-t border-gray-200 dark:border-gray-700">
                                <div class="flex items-center justify-between">
                                    <span class="text-sm text-gray-500 dark:text-gray-400">Trend</span>
                                    <div class="flex items-center space-x-2">
                                        <div class="w-2 h-2 bg-red-500 rounded-full"></div>
                                        <span class="text-sm font-medium text-red-600 dark:text-red-400">Bearish</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Action Buttons -->
                <div class="flex flex-col sm:flex-row gap-4 justify-center mt-8">
                    <button
                        class="bg-gradient-to-r from-green-600 to-green-700 text-white px-8 py-3 rounded-xl font-semibold hover:shadow-lg transform hover:scale-105 transition-all duration-200 flex items-center justify-center">
                        <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15">
                            </path>
                        </svg>
                        Refresh Prices
                    </button>
                    <button
                        class="bg-white text-green-600 border-2 border-green-600 px-8 py-3 rounded-xl font-semibold hover:bg-green-50 transform hover:scale-105 transition-all duration-200 flex items-center justify-center">
                        <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z">
                            </path>
                        </svg>
                        View All Commodities
                    </button>
                </div>
            </section>

            <!-- Market Activity Table -->
            <section
                class="bg-white/70 dark:bg-gray-900/60 backdrop-blur-sm rounded-2xl p-8 shadow-xl border border-amber-100 dark:border-gray-800">
                <h2 class="text-3xl md:text-4xl font-bold mb-8 text-center text-gray-800 dark:text-gray-100">Market
                    Activity
                </h2>
                <div class="bg-white dark:bg-gray-900 py-10">
                    <h3 class="px-4 text-base/7 font-semibold text-gray-900 dark:text-gray-100 sm:px-6 lg:px-8">Latest
                        Price
                        Updates</h3>
                    <table class="mt-6 w-full text-left whitespace-nowrap text-gray-900 dark:text-gray-100">
                        <colgroup>
                            <col class="w-full sm:w-4/12" />
                            <col class="lg:w-4/12" />
                            <col class="lg:w-2/12" />
                            <col class="lg:w-1/12" />
                            <col class="lg:w-1/12" />
                        </colgroup>
                        <thead
                            class="border-b border-gray-200 dark:border-gray-700 text-sm/6 text-gray-900 dark:text-gray-100">
                            <tr>
                                <th scope="col" class="py-2 pr-8 pl-4 font-semibold sm:pl-6 lg:pl-8">Commodity</th>
                                <th scope="col" class="hidden py-2 pr-8 pl-0 font-semibold sm:table-cell">Market</th>
                                <th scope="col"
                                    class="py-2 pr-4 pl-0 text-right font-semibold sm:pr-8 sm:text-left lg:pr-20">
                                    Status
                                </th>
                                <th scope="col" class="hidden py-2 pr-8 pl-0 font-semibold md:table-cell lg:pr-20">
                                    Change</th>
                                <th scope="col"
                                    class="hidden py-2 pr-4 pl-0 text-right font-semibold sm:table-cell sm:pr-6 lg:pr-8">
                                    Updated</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-gray-100 dark:divide-gray-800">
                            <tr>
                                <td class="py-4 pr-8 pl-4 sm:pl-6 lg:pl-8">
                                    <div class="flex items-center gap-x-4">
                                        <div
                                            class="w-8 h-8 bg-gradient-to-br from-yellow-400 to-yellow-500 rounded-full flex items-center justify-center">
                                            <svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 24 24">
                                                <path
                                                    d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                            </svg>
                                        </div>
                                        <div class="truncate text-sm/6 font-medium text-gray-900 dark:text-gray-100">
                                            Maize</div>
                                    </div>
                                </td>
                                <td class="hidden py-4 pr-4 pl-0 sm:table-cell sm:pr-8">
                                    <div class="flex gap-x-3">
                                        <div class="font-mono text-sm/6 text-gray-500 dark:text-gray-400">Kumasi Central
                                        </div>
                                        <div
                                            class="rounded-md bg-green-100 dark:bg-green-900/30 px-2 py-1 text-xs font-medium text-green-600 dark:text-green-400 outline-1 outline-green-200 dark:outline-green-800">
                                            Active</div>
                                    </div>
                                </td>
                                <td class="py-4 pr-4 pl-0 text-sm/6 sm:pr-8 lg:pr-20">
                                    <div class="flex items-center justify-end gap-x-2 sm:justify-start">
                                        <time datetime="2023-01-23T11:00"
                                            class="text-gray-500 dark:text-gray-400 sm:hidden">45
                                            minutes
                                            ago</time>
                                        <div
                                            class="flex-none rounded-full bg-green-600/10 dark:bg-green-400/20 p-1 text-green-600 dark:text-green-400">
                                            <div class="size-1.5 rounded-full bg-current"></div>
                                        </div>
                                        <div class="hidden text-gray-900 dark:text-gray-100 sm:block">Rising</div>
                                    </div>
                                </td>
                                <td
                                    class="hidden py-4 pr-8 pl-0 text-sm/6 text-green-600 dark:text-green-400 md:table-cell lg:pr-20">
                                    +₵12
                                </td>
                                <td
                                    class="hidden py-4 pr-4 pl-0 text-right text-sm/6 text-gray-500 dark:text-gray-400 sm:table-cell sm:pr-6 lg:pr-8">
                                    <time datetime="2023-01-23T11:00">45 minutes ago</time>
                                </td>
                            </tr>
                            <tr>
                                <td class="py-4 pr-8 pl-4 sm:pl-6 lg:pl-8">
                                    <div class="flex items-center gap-x-4">
                                        <div
                                            class="w-8 h-8 bg-gradient-to-br from-amber-400 to-amber-500 rounded-full flex items-center justify-center">
                                            <svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 24 24">
                                                <path
                                                    d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                            </svg>
                                        </div>
                                        <div class="truncate text-sm/6 font-medium text-gray-900 dark:text-gray-100">
                                            Rice</div>
                                    </div>
                                </td>
                                <td class="hidden py-4 pr-4 pl-0 sm:table-cell sm:pr-8">
                                    <div class="flex gap-x-3">
                                        <div class="font-mono text-sm/6 text-gray-500 dark:text-gray-400">Accra Market
                                        </div>
                                        <div
                                            class="rounded-md bg-green-100 dark:bg-green-900/30 px-2 py-1 text-xs font-medium text-green-600 dark:text-green-400 outline-1 outline-green-200 dark:outline-green-800">
                                            Active</div>
                                    </div>
                                </td>
                                <td class="py-4 pr-4 pl-0 text-sm/6 sm:pr-8 lg:pr-20">
                                    <div class="flex items-center justify-end gap-x-2 sm:justify-start">
                                        <time datetime="2023-01-23T09:00"
                                            class="text-gray-500 dark:text-gray-400 sm:hidden">3
                                            hours
                                            ago</time>
                                        <div
                                            class="flex-none rounded-full bg-green-600/10 dark:bg-green-400/20 p-1 text-green-600 dark:text-green-400">
                                            <div class="size-1.5 rounded-full bg-current"></div>
                                        </div>
                                        <div class="hidden text-gray-900 dark:text-gray-100 sm:block">Rising</div>
                                    </div>
                                </td>
                                <td
                                    class="hidden py-4 pr-8 pl-0 text-sm/6 text-green-600 dark:text-green-400 md:table-cell lg:pr-20">
                                    +₵8
                                </td>
                                <td
                                    class="hidden py-4 pr-4 pl-0 text-right text-sm/6 text-gray-500 dark:text-gray-400 sm:table-cell sm:pr-6 lg:pr-8">
                                    <time datetime="2023-01-23T09:00">3 hours ago</time>
                                </td>
                            </tr>
                            <tr>
                                <td class="py-4 pr-8 pl-4 sm:pl-6 lg:pl-8">
                                    <div class="flex items-center gap-x-4">
                                        <div
                                            class="w-8 h-8 bg-gradient-to-br from-green-400 to-green-500 rounded-full flex items-center justify-center">
                                            <svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 24 24">
                                                <path
                                                    d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                            </svg>
                                        </div>
                                        <div class="truncate text-sm/6 font-medium text-gray-900 dark:text-gray-100">
                                            Beans</div>
                                    </div>
                                </td>
                                <td class="hidden py-4 pr-4 pl-0 sm:table-cell sm:pr-8">
                                    <div class="flex gap-x-3">
                                        <div class="font-mono text-sm/6 text-gray-500 dark:text-gray-400">Tamale Market
                                        </div>
                                        <div
                                            class="rounded-md bg-red-100 dark:bg-red-900/30 px-2 py-1 text-xs font-medium text-red-600 dark:text-red-400 outline-1 outline-red-200 dark:outline-red-800">
                                            Volatile</div>
                                    </div>
                                </td>
                                <td class="py-4 pr-4 pl-0 text-sm/6 sm:pr-8 lg:pr-20">
                                    <div class="flex items-center justify-end gap-x-2 sm:justify-start">
                                        <time datetime="2023-01-23T00:00"
                                            class="text-gray-500 dark:text-gray-400 sm:hidden">12
                                            hours
                                            ago</time>
                                        <div
                                            class="flex-none rounded-full bg-rose-600/10 dark:bg-red-400/20 p-1 text-rose-600 dark:text-red-400">
                                            <div class="size-1.5 rounded-full bg-current"></div>
                                        </div>
                                        <div class="hidden text-gray-900 dark:text-gray-100 sm:block">Falling</div>
                                    </div>
                                </td>
                                <td
                                    class="hidden py-4 pr-8 pl-0 text-sm/6 text-red-600 dark:text-red-400 md:table-cell lg:pr-20">
                                    -₵15
                                </td>
                                <td
                                    class="hidden py-4 pr-4 pl-0 text-right text-sm/6 text-gray-500 dark:text-gray-400 sm:table-cell sm:pr-6 lg:pr-8">
                                    <time datetime="2023-01-23T00:00">12 hours ago</time>
                                </td>
                            </tr>
                            <tr>
                                <td class="py-4 pr-8 pl-4 sm:pl-6 lg:pl-8">
                                    <div class="flex items-center gap-x-4">
                                        <div
                                            class="w-8 h-8 bg-gradient-to-br from-orange-400 to-orange-500 rounded-full flex items-center justify-center">
                                            <svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 24 24">
                                                <path
                                                    d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                            </svg>
                                        </div>
                                        <div class="truncate text-sm/6 font-medium text-gray-900 dark:text-gray-100">
                                            Cassava
                                        </div>
                                    </div>
                                </td>
                                <td class="hidden py-4 pr-4 pl-0 sm:table-cell sm:pr-8">
                                    <div class="flex gap-x-3">
                                        <div class="font-mono text-sm/6 text-gray-500 dark:text-gray-400">Cape Coast
                                        </div>
                                        <div
                                            class="rounded-md bg-green-100 dark:bg-green-900/30 px-2 py-1 text-xs font-medium text-green-600 dark:text-green-400 outline-1 outline-green-200 dark:outline-green-800">
                                            Active</div>
                                    </div>
                                </td>
                                <td class="py-4 pr-4 pl-0 text-sm/6 sm:pr-8 lg:pr-20">
                                    <div class="flex items-center justify-end gap-x-2 sm:justify-start">
                                        <time datetime="2023-01-21T13:00"
                                            class="text-gray-500 dark:text-gray-400 sm:hidden">2
                                            days
                                            ago</time>
                                        <div
                                            class="flex-none rounded-full bg-green-600/10 dark:bg-green-400/20 p-1 text-green-600 dark:text-green-400">
                                            <div class="size-1.5 rounded-full bg-current"></div>
                                        </div>
                                        <div class="hidden text-gray-900 dark:text-gray-100 sm:block">Stable</div>
                                    </div>
                                </td>
                                <td
                                    class="hidden py-4 pr-8 pl-0 text-sm/6 text-gray-600 dark:text-gray-400 md:table-cell lg:pr-20">
                                    +₵2
                                </td>
                                <td
                                    class="hidden py-4 pr-4 pl-0 text-right text-sm/6 text-gray-500 dark:text-gray-400 sm:table-cell sm:pr-6 lg:pr-8">
                                    <time datetime="2023-01-21T13:00">2 days ago</time>
                                </td>
                            </tr>
                            <tr>
                                <td class="py-4 pr-8 pl-4 sm:pl-6 lg:pl-8">
                                    <div class="flex items-center gap-x-4">
                                        <div
                                            class="w-8 h-8 bg-gradient-to-br from-purple-400 to-purple-500 rounded-full flex items-center justify-center">
                                            <svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 24 24">
                                                <path
                                                    d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                            </svg>
                                        </div>
                                        <div class="truncate text-sm/6 font-medium text-gray-900 dark:text-gray-100">Yam
                                        </div>
                                    </div>
                                </td>
                                <td class="hidden py-4 pr-4 pl-0 sm:table-cell sm:pr-8">
                                    <div class="flex gap-x-3">
                                        <div class="font-mono text-sm/6 text-gray-500 dark:text-gray-400">Koforidua
                                        </div>
                                        <div
                                            class="rounded-md bg-green-100 dark:bg-green-900/30 px-2 py-1 text-xs font-medium text-green-600 dark:text-green-400 outline-1 outline-green-200 dark:outline-green-800">
                                            Active</div>
                                    </div>
                                </td>
                                <td class="py-4 pr-4 pl-0 text-sm/6 sm:pr-8 lg:pr-20">
                                    <div class="flex items-center justify-end gap-x-2 sm:justify-start">
                                        <time datetime="2023-01-18T12:34"
                                            class="text-gray-500 dark:text-gray-400 sm:hidden">5
                                            days
                                            ago</time>
                                        <div
                                            class="flex-none rounded-full bg-green-600/10 dark:bg-green-400/20 p-1 text-green-600 dark:text-green-400">
                                            <div class="size-1.5 rounded-full bg-current"></div>
                                        </div>
                                        <div class="hidden text-gray-900 dark:text-gray-100 sm:block">Rising</div>
                                    </div>
                                </td>
                                <td
                                    class="hidden py-4 pr-8 pl-0 text-sm/6 text-green-600 dark:text-green-400 md:table-cell lg:pr-20">
                                    +₵25
                                </td>
                                <td
                                    class="hidden py-4 pr-4 pl-0 text-right text-sm/6 text-gray-500 dark:text-gray-400 sm:table-cell sm:pr-6 lg:pr-8">
                                    <time datetime="2023-01-18T12:34">5 days ago</time>
                                </td>
                            </tr>
                            <tr>
                                <td class="py-4 pr-8 pl-4 sm:pl-6 lg:pl-8">
                                    <div class="flex items-center gap-x-4">
                                        <div
                                            class="w-8 h-8 bg-gradient-to-br from-lime-400 to-lime-500 rounded-full flex items-center justify-center">
                                            <svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 24 24">
                                                <path
                                                    d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z" />
                                            </svg>
                                        </div>
                                        <div class="truncate text-sm/6 font-medium text-gray-900 dark:text-gray-100">
                                            Plantain
                                        </div>
                                    </div>
                                </td>
                                <td class="hidden py-4 pr-4 pl-0 sm:table-cell sm:pr-8">
                                    <div class="flex gap-x-3">
                                        <div class="font-mono text-sm/6 text-gray-500 dark:text-gray-400">Takoradi</div>
                                        <div
                                            class="rounded-md bg-yellow-100 dark:bg-yellow-900/30 px-2 py-1 text-xs font-medium text-yellow-600 dark:text-yellow-400 outline-1 outline-yellow-200 dark:outline-yellow-800">
                                            Seasonal</div>
                                    </div>
                                </td>
                                <td class="py-4 pr-4 pl-0 text-sm/6 sm:pr-8 lg:pr-20">
                                    <div class="flex items-center justify-end gap-x-2 sm:justify-start">
                                        <time datetime="2023-01-16T15:54"
                                            class="text-gray-500 dark:text-gray-400 sm:hidden">1
                                            week
                                            ago</time>
                                        <div
                                            class="flex-none rounded-full bg-green-600/10 dark:bg-green-400/20 p-1 text-green-600 dark:text-green-400">
                                            <div class="size-1.5 rounded-full bg-current"></div>
                                        </div>
                                        <div class="hidden text-gray-900 dark:text-gray-100 sm:block">Stable</div>
                                    </div>
                                </td>
                                <td
                                    class="hidden py-4 pr-8 pl-0 text-sm/6 text-gray-600 dark:text-gray-400 md:table-cell lg:pr-20">
                                    +₵5
                                </td>
                                <td
                                    class="hidden py-4 pr-4 pl-0 text-right text-sm/6 text-gray-500 dark:text-gray-400 sm:table-cell sm:pr-6 lg:pr-8">
                                    <time datetime="2023-01-16T15:54">1 week ago</time>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </section>

            <!-- Data & Analytics -->
            <section
                class="bg-white dark:bg-gray-900 rounded-3xl p-8 shadow-lg border border-gray-100 dark:border-gray-800">
                <div class="flex items-center justify-between mb-6">
                    <h2 class="text-2xl md:text-3xl font-bold text-gray-900 dark:text-gray-100">Data & Analytics</h2>
                    <div class="text-sm text-gray-500 dark:text-gray-400">Updated 2 min ago</div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Price History (placeholder mini chart bars) -->
                    <div
                        class="rounded-2xl border border-gray-100 dark:border-gray-800 p-6 bg-gray-50 dark:bg-gray-800">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold text-gray-800 dark:text-gray-100">Price History</h3>
                            <span class="text-xs text-gray-500 dark:text-gray-400">30 days</span>
                        </div>
                        <div class="h-28 bg-white dark:bg-gray-900 rounded-lg p-3 flex items-end gap-1">
                            <div class="w-2 bg-green-400 rounded-t" style="height: 24%"></div>
                            <div class="w-2 bg-green-400 rounded-t" style="height: 38%"></div>
                            <div class="w-2 bg-green-400 rounded-t" style="height: 52%"></div>
                            <div class="w-2 bg-green-400 rounded-t" style="height: 44%"></div>
                            <div class="w-2 bg-green-500 rounded-t" style="height: 70%"></div>
                            <div class="w-2 bg-green-500 rounded-t" style="height: 76%"></div>
                            <div class="w-2 bg-green-600 rounded-t" style="height: 88%"></div>
                            <div class="w-2 bg-green-500 rounded-t" style="height: 72%"></div>
                            <div class="w-2 bg-green-400 rounded-t" style="height: 60%"></div>
                            <div class="w-2 bg-green-600 rounded-t" style="height: 92%"></div>
                        </div>
                        <div class="mt-3 flex justify-between text-xs text-gray-500 dark:text-gray-400">
                            <span>Start</span>
                            <span>Today</span>
                        </div>
                    </div>

                    <!-- Market Comparison -->
                    <div
                        class="rounded-2xl border border-gray-100 dark:border-gray-800 p-6 bg-gray-50 dark:bg-gray-800">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold text-gray-800 dark:text-gray-100">Market Comparison</h3>
                            <span class="text-xs text-gray-500 dark:text-gray-400">Maize</span>
                        </div>
                        <div class="space-y-3">
                            <div class="flex items-center justify-between text-sm">
                                <span class="text-gray-600 dark:text-gray-300">Accra</span>
                                <span class="font-semibold text-gray-900 dark:text-gray-100">₵470</span>
                            </div>
                            <div class="flex items-center justify-between text-sm">
                                <span class="text-gray-600 dark:text-gray-300">Kumasi</span>
                                <span class="font-semibold text-gray-900 dark:text-gray-100">₵455</span>
                            </div>
                            <div class="flex items-center justify-between text-sm">
                                <span class="text-gray-600 dark:text-gray-300">Tamale</span>
                                <span class="font-semibold text-gray-900 dark:text-gray-100">₵440</span>
                            </div>
                            <div class="flex items-center justify-between text-sm">
                                <span class="text-gray-600 dark:text-gray-300">Takoradi</span>
                                <span class="font-semibold text-gray-900 dark:text-gray-100">₵462</span>
                            </div>
                        </div>
                        <div class="mt-4 h-2 bg-white dark:bg-gray-900 rounded-full overflow-hidden">
                            <div class="h-full bg-green-500 w-3/4"></div>
                        </div>
                    </div>

                    <!-- Seasonal Trends -->
                    <div
                        class="rounded-2xl border border-gray-100 dark:border-gray-800 p-6 bg-gray-50 dark:bg-gray-800">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold text-gray-800 dark:text-gray-100">Seasonal Trends</h3>
                            <span class="text-xs text-gray-500 dark:text-gray-400">12 months</span>
                        </div>
                        <div class="grid grid-cols-4 gap-2 text-xs">
                            <div class="p-3 rounded-xl bg-white dark:bg-gray-900 text-center">
                                <div class="font-semibold text-gray-900 dark:text-gray-100">Jan</div>
                                <div class="text-green-600 dark:text-green-400">+3%</div>
                            </div>
                            <div class="p-3 rounded-xl bg-white dark:bg-gray-900 text-center">
                                <div class="font-semibold text-gray-900 dark:text-gray-100">Apr</div>
                                <div class="text-green-600 dark:text-green-400">+5%</div>
                            </div>
                            <div class="p-3 rounded-xl bg-white dark:bg-gray-900 text-center">
                                <div class="font-semibold text-gray-900 dark:text-gray-100">Aug</div>
                                <div class="text-red-600 dark:text-red-400">-2%</div>
                            </div>
                            <div class="p-3 rounded-xl bg-white dark:bg-gray-900 text-center">
                                <div class="font-semibold text-gray-900 dark:text-gray-100">Dec</div>
                                <div class="text-green-600 dark:text-green-400">+6%</div>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Market Intelligence -->
            <section
                class="bg-white dark:bg-gray-900 rounded-3xl p-8 shadow-lg border border-gray-100 dark:border-gray-800">
                <div class="flex items-center justify-between mb-6">
                    <h2 class="text-2xl md:text-3xl font-bold text-gray-900 dark:text-gray-100">Market Intelligence</h2>
                    <a href="#"
                        class="text-sm text-green-700 dark:text-green-400 hover:text-green-800 dark:hover:text-green-300">View
                        reports</a>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Weather Impact -->
                    <div
                        class="rounded-2xl border border-gray-100 dark:border-gray-800 p-6 bg-gray-50 dark:bg-gray-800">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold text-gray-800 dark:text-gray-100">Weather Impact</h3>
                            <span class="text-xs text-gray-500 dark:text-gray-400">This week</span>
                        </div>
                        <div class="space-y-3 text-sm">
                            <div class="flex items-center justify-between">
                                <span class="text-gray-600 dark:text-gray-300">Rainfall (mm)</span>
                                <span class="font-semibold text-gray-900 dark:text-gray-100">24</span>
                            </div>
                            <div class="flex items-center justify-between">
                                <span class="text-gray-600 dark:text-gray-300">Temp (°C)</span>
                                <span class="font-semibold text-gray-900 dark:text-gray-100">30</span>
                            </div>
                            <div class="flex items-center justify-between">
                                <span class="text-gray-600 dark:text-gray-300">Impact</span>
                                <span class="font-semibold text-amber-600">Moderate</span>
                            </div>
                        </div>
                        <div class="mt-4 h-2 bg-white dark:bg-gray-900 rounded-full overflow-hidden">
                            <div class="h-full bg-amber-500 w-2/3"></div>
                        </div>
                    </div>

                    <!-- News Feed -->
                    <div
                        class="rounded-2xl border border-gray-100 dark:border-gray-800 p-6 bg-gray-50 dark:bg-gray-800">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold text-gray-800 dark:text-gray-100">News Feed</h3>
                            <span class="text-xs text-gray-500 dark:text-gray-400">Ghana</span>
                        </div>
                        <ul class="space-y-3 text-sm">
                            <li
                                class="p-3 bg-white dark:bg-gray-900 rounded-xl border border-gray-100 dark:border-gray-800 text-gray-800 dark:text-gray-100">
                                New maize harvest expected to
                                stabilize prices in northern regions.</li>
                            <li
                                class="p-3 bg-white dark:bg-gray-900 rounded-xl border border-gray-100 dark:border-gray-800 text-gray-800 dark:text-gray-100">
                                Import duties adjusted for rice;
                                traders
                                anticipate mild price changes.</li>
                            <li
                                class="p-3 bg-white dark:bg-gray-900 rounded-xl border border-gray-100 dark:border-gray-800 text-gray-800 dark:text-gray-100">
                                Transport costs fall 5%; improved
                                margins for coastal markets.</li>
                        </ul>
                    </div>

                    <!-- Market Reports -->
                    <div
                        class="rounded-2xl border border-gray-100 dark:border-gray-800 p-6 bg-gray-50 dark:bg-gray-800">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-semibold text-gray-800 dark:text-gray-100">Market Reports</h3>
                            <span class="text-xs text-gray-500 dark:text-gray-400">Weekly</span>
                        </div>
                        <div class="space-y-3 text-sm">
                            <div class="flex items-center justify-between">
                                <span class="text-gray-600 dark:text-gray-300">Top Rising</span>
                                <span class="font-semibold text-green-600">Yam +4.1%</span>
                            </div>
                            <div class="flex items-center justify-between">
                                <span class="text-gray-600 dark:text-gray-300">Top Falling</span>
                                <span class="font-semibold text-red-600">Beans -1.5%</span>
                            </div>
                            <div class="flex items-center justify-between">
                                <span class="text-gray-600 dark:text-gray-300">Volatility</span>
                                <span class="font-semibold text-amber-600">Medium</span>
                            </div>
                        </div>
                        <button
                            class="mt-4 w-full text-center px-4 py-2 rounded-lg border border-gray-200 dark:border-gray-700 text-gray-700 dark:text-gray-100 hover:bg-gray-100 dark:hover:bg-gray-700">Download
                            PDF</button>
                    </div>
                </div>
            </section>
        </main>

        <!-- Footer -->
        <footer class="bg-white dark:bg-gray-900 border-t border-gray-200 dark:border-gray-800">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="py-8 flex flex-col sm:flex-row items-center justify-between gap-4">
                    <p class="text-sm text-gray-600 dark:text-gray-300">A product of <span
                            class="font-semibold text-gray-900 dark:text-white">WeOrg</span></p>
                    <p class="text-xs text-gray-500 dark:text-gray-400">© 2025 Boafo</p>
                </div>
            </div>
        </footer>
    </div>
</template>

<style scoped></style>
