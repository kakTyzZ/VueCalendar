<script setup>
import { ref, reactive, computed } from 'vue'

const Data = reactive([
  {
    eng: 'January',
    ru: 'Январь',
    shortEng: 'Jan',
    shortRu: 'Янв',
    number: 1,
    days: 31
  },
  {
    eng: 'February',
    ru: 'Февраль',
    shortEng: 'Feb',
    shortRu: 'Фев',
    number: 2,
    days: 28
  },
  {
    eng: 'March',
    ru: 'Март',
    shortEng: 'Mar',
    shortRu: 'Мар',
    number: 3,
    days: 31
  },
  {
    eng: 'April',
    ru: 'Апрель',
    shortEng: 'Apr',
    shortRu: 'Апр',
    number: 4,
    days: 30
  },
  {
    eng: 'May',
    ru: 'Май',
    shortEng: 'May',
    shortRu: 'Май',
    number: 5,
    days: 31
  },
  {
    eng: 'June',
    ru: 'Июнь',
    shortEng: 'Jun',
    shortRu: 'Июн',
    number: 6,
    days: 30
  },
  {
    eng: 'July',
    ru: 'Июль',
    shortEng: 'Jul',
    shortRu: 'Июл',
    number: 7,
    days: 31
  },
  {
    eng: 'August',
    ru: 'Август',
    shortEng: 'Aug',
    shortRu: 'Авг',
    number: 8,
    days: 31
  },
  {
    eng: 'September',
    ru: 'Сентябрь',
    shortEng: 'Sep',
    shortRu: 'Сен',
    number: 9,
    days: 30
  },
  {
    eng: 'October',
    ru: 'Октябрь',
    shortEng: 'Oct',
    shortRu: 'Окт',
    number: 10,
    days: 31
  },
  {
    eng: 'November',
    ru: 'Ноябрь',
    shortEng: 'Nov',
    shortRu: 'Ноя',
    number: 11,
    days: 30
  },
  {
    eng: 'December',
    ru: 'Декабрь',
    shortEng: 'Dec',
    shortRu: 'Дек',
    number: 12,
    days: 31
  }
])

const daysOfTheWeek = ref(['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'])
const daysOfTheWeekRu = ref(['Пон', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'])
const currentWeek = computed(() =>
  currentLanguage.value === 'shortRu' ? daysOfTheWeekRu.value : daysOfTheWeek.value
)

const currentLanguage = ref('shortRu')
const myDate = new Date()
const currentDate = reactive({
  year: myDate.getFullYear(),
  month: myDate.getMonth(),
  day: myDate.getDate()
})
const inputData = ref('')

const currentMonth = computed(() => Data[currentDate.month].days)

function inputHandler() {
  const splitDate = inputData.value.split('-')
  currentDate.year = splitDate[0]

  if (+splitDate[1][0] === 0) {
    currentDate.month = splitDate[1][1]
  } else {
    currentDate.month = splitDate[1]
  }

  currentDate.day = splitDate[2][0] === 0 ? splitDate[2][1] : splitDate[2]
}

function nextMonth() {
  if (currentDate.month === 11) {
    currentDate.month = 0
    currentDate.year++
  } else {
    currentDate.month++
  }
}
function previousMonth() {
  if (currentDate.month === 0) {
    currentDate.month = 11
    currentDate.year--
  } else {
    currentDate.month--
  }
}

function onClickDay(e) {
  currentDate.day = e.target.innerText
  console.log(currentDate)
  return currentDate
}

function onChangeLanguageClick() {
  if (currentLanguage.value === 'shortRu') {
    currentLanguage.value = 'shortEng'
  } else {
    currentLanguage.value = 'shortRu'
  }
}
</script>

<template>
  <div class="h-screen w-full flex flex-col items-center justify-center">
    <div class="flex flex-col border-2">
      <div class="flex max-w-[600px] w-full justify-between mx-auto p-3">
        <div @click="previousMonth" class="max-w-[20px] w-full">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
            <path
              d="M9.4 233.4c-12.5 12.5-12.5 32.8 0 45.3l160 160c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L109.2 288 416 288c17.7 0 32-14.3 32-32s-14.3-32-32-32l-306.7 0L214.6 118.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0l-160 160z"
            />
          </svg>
        </div>
        <div>{{ Data[currentDate.month][currentLanguage] }}, {{ currentDate.year }}</div>
        <div @click="nextMonth" class="max-w-[20px] w-full rotate-180">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
            <path
              d="M9.4 233.4c-12.5 12.5-12.5 32.8 0 45.3l160 160c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L109.2 288 416 288c17.7 0 32-14.3 32-32s-14.3-32-32-32l-306.7 0L214.6 118.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0l-160 160z"
            />
          </svg>
        </div>
      </div>
      <div class="flex min-w-[600px] w-full mx-auto">
        <div class="border-1 p-3 text-center w-full" v-for="i in currentWeek" :key="i">
          <div>
            {{ i }}
          </div>
        </div>
      </div>
      <div class="grid grid-cols-7 grid-rows-5 border max-w-[600px] w-full mx-auto">
        <div
          class="border-1 p-3 text-center bg-white w-full"
          v-for="i in currentMonth"
          :class="i === +currentDate.day ? 'border-sky-700 border-2' : ''"
          :key="i"
          @click="onClickDay"
        >
          <div>
            {{ i }}
          </div>
        </div>
      </div>
    </div>

    <div class="flex m-5 p-3">
      <input class="w-full border-2 rounded-l-xl p-1" type="text" v-model="inputData" />
      <button
        class="border-2 px-3 hover:bg-sky-400 rounded-r-xl cursor-pointer"
        @click="inputHandler"
      >
        Click
      </button>
    </div>

    <div class="text-2xl">
      Year: {{ currentDate.year }}, Month: {{ currentDate.month }}, Day: {{ currentDate.day }}
    </div>

    <div
      class="text-2xl px-3 rounded-xl border-2 border-sky-200 mt-2 hover:bg-sky-400 cursor-pointer"
      @click="onChangeLanguageClick"
    >
      {{ currentLanguage === 'shortRu' ? 'Сменить Язык' : 'Change language' }}
    </div>
  </div>
</template>

<style scoped></style>
