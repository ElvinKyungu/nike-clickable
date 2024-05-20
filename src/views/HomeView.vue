
<script setup lang="ts">
import { ref, watch, nextTick } from 'vue'
import IconStars from '@/components/icons/IconStars.vue'
import IconStarsTransparent from '@/components/icons/IconStarsTransparent.vue'
import IconLeft from '@/components/icons/IconLeft.vue'
import IconDown from '@/components/icons/IconDown.vue'
import IconRight from '@/components/icons/IconRight.vue'
import Input from '@/components/Input.vue'
import gsap from 'gsap'

import shoe1 from '@/assets/shoe1.png'
import shoe2 from '@/assets/shoe2.png'
import shoe3 from '@/assets/shoe3.png'
import shoe4 from '@/assets/shoe4.png'

const images = [shoe1, shoe2, shoe3, shoe4]
const shoeTitles = ["Nike Dunk Low Next Nature SE", "Nike Air Max 90", "Adidas Ultraboost", "Puma RS-X"]
const shoePrices = ["$120", "$150", "$180", "$140"]

const showDetails = ref(false)
const showCard = ref(true)
const showSecondCard = ref(false)
const currentImageIndex = ref(0)
const currentShoe = ref({ title: shoeTitles[0], price: shoePrices[0], image: images[0] })

const handleBuyNow = () => {
  showCard.value = false
  showSecondCard.value = true
}

const handleClickOutside = () => {
  gsap.to('.second-card', {
    y: '100%',
    opacity: 0,
    duration: 0.5,
    ease: 'power3.in',
    onComplete: () => {
      showSecondCard.value = false
      showCard.value = true
    }
  })
}

const animateSecondCard = () => {
  nextTick(() => {
    gsap.fromTo(
      '.second-card',
      { y: '100%', opacity: 0 },
      { y: '0%', opacity: 1, duration: 0.5, ease: 'power3.out' }
    )
  })
}

watch(showSecondCard, (newVal) => {
  if (newVal) animateSecondCard()
})

const animateText = () => {
  gsap.to(['.shoe-title', '.shoe-price'], {
    y: 0,
    opacity: 1,
    duration: 0.5,
    ease: 'power3.out',
    startAt: { y: '100%', opacity: 0 }
  })
}

const changeImage = (direction: 'left' | 'right') => {
  const totalImages = images.length
  const nextIndex = direction === 'right'
    ? (currentImageIndex.value + 1) % totalImages
    : (currentImageIndex.value - 1 + totalImages) % totalImages

  gsap.to('.shoe-image', {
    x: direction === 'right' ? '-100%' : '100%',
    opacity: 0,
    duration: 0.5,
    ease: 'power3.in',
    onComplete: () => {
      currentImageIndex.value = nextIndex
      currentShoe.value = {
        title: shoeTitles[nextIndex],
        price: shoePrices[nextIndex],
        image: images[nextIndex]
      }
      gsap.fromTo(
        '.shoe-image',
        { x: direction === 'right' ? '100%' : '-100%', opacity: 0 },
        { x: '0%', opacity: 1, duration: 0.5, ease: 'power3.out' }
      )
      animateText()
    }
  })
}
</script>
<template>
  <main class="flex justify-center items-center w-full h-screen px-4 md:px-0">
    <div v-if="showSecondCard" title="click on it to close" @click="handleClickOutside" class="fixed z-0 w-full h-screen cursor-pointer"></div>
    <div v-if="showCard" class="px-4 md:px-10 rounded-lg min-h-[35rem] min-w-[20rem] md:min-w-[30rem] bg-[#f4f3f4] relative border border-gray-200">
      <div class="absolute h-[15rem] md:h-[20rem] w-full">
        <img src="@/assets/shoe1.png" class="w-full h-full object-contain z-10 right-10 transition duration-200 cursor-pointer hover:scale-150 absolute" alt="">
      </div>
      <div class="absolute bottom-0 min-h-40 w-full bg-white py-10 left-0 px-4 md:px-10">
        <h4 class="text-xl md:text-2xl font-medium">Nike Dunk Low Next Nature SE</h4>
        <button @click="handleBuyNow" class="mt-4 bg-black w-full py-3 md:py-4 text-white rounded-lg text-lg md:text-xl font-medium">Buy now</button>
      </div>
    </div>

    <div v-if="showSecondCard" class="flex flex-col md:flex-row w-full h-auto md:h-[80vh] mx-4 md:mx-56 bg-[#f4f3f4] relative border border-gray-200 shadow-xl second-card">
      <div class="w-full h-full md:w-[65%] px-4 md:px-10 pt-10 flex flex-col justify-center relative">
        <div class="flex justify-between text-xl md:text-2xl font-medium absolute top-10 pr-4 md:pr-20 w-full">
          <span class="shoe-title">{{ currentShoe.title }}</span>
          <span class="shoe-price">{{ currentShoe.price }}</span>
          <span @click="handleClickOutside" class="bg-black/10 font-light cursor-pointer h-10 flex justify-center w-10 rounded-full">x</span>
        </div>
        <div class="flex justify-between items-center">
          <IconLeft @click="changeImage('left')" />
          <img :src="currentShoe.image" class="shoe-image w-full md:w-[90%] h-[50vh] md:h-[80vh] object-contain" alt="">
          <IconRight @click="changeImage('right')" />
        </div>
      </div>
      <div class="w-full h-full bg-white md:w-[35%] relative mt-4 md:mt-0">
        <div class="absolute w-full h-full bg-white p-4 md:p-10">
          <span class="text-lg md:text-xl font-medium">Colors</span>
          <div class="flex gap-2 justify-start mt-5">
            <div class="colors w-8 h-8 rounded-full bg-[#D571D5]"></div>
            <div class="colors w-8 h-8 rounded-full bg-green-700"></div>
            <div class="colors w-8 h-8 rounded-full bg-orange-600"></div>
            <div class="colors w-8 h-8 rounded-full bg-red-700"></div>
            <div class="colors w-8 h-8 rounded-full bg-black"></div>
          </div>

          <div class="mt-7">
            <span class="text-lg md:text-xl font-medium">Select Size</span>
            <div class="grid grid-cols-4 gap-4 flex-wrap mt-5">
              <Input />
              <Input />
              <Input />
              <Input />
              <Input />
              <Input />
              <Input />
              <Input />
            </div>
            <div class="flex items-center justify-between mt-7">
              <span class="text-lg md:text-xl font-medium">Avis</span>
              <div class="flex gap-2">
                <IconStars />
                <IconStars />
                <IconStars />
                <IconStarsTransparent />
              </div>
            </div>
            <div class="mt-7">
              <div class="flex justify-between items-center border-b border-gray-200 pb-4">
                <h4 class="text-lg md:text-xl font-medium">Details</h4>
                <IconDown @click="showDetails = !showDetails" />
              </div>
              <transition>
                <p v-if="showDetails" class="text-base md:text-lg text-gray-600 mt-5 transition">
                  Combine style and comfort with this Nike shoe.
                  Modern and high-performance, perfect for sports and everyday wear.
                </p>
              </transition>
              <button class="mt-7 bg-black w-full py-3 md:py-4 text-white rounded-lg text-lg md:text-xl font-medium">Add to cart</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.icon-rotate:hover {
  transform: rotate(360deg);
  transition: .3s ease-in-out;
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.shoe-title, .shoe-price {
  display: inline-block;
  transition: transform 0.5s, opacity 0.5s;
}
</style>
