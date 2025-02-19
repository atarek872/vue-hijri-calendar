<template>
    <div class="calendar-container">
      <div class="calendar-header">
        <button @click="prevMonth">←</button>
        <h2>{{ currentDate.format(getHeaderFormat()) }}</h2>
        <button @click="nextMonth">→</button>
      </div>
  
      <div class="calendar-switcher">
        <button @click="switchCalendar('gregorian')" :class="{ active: calendarType === 'gregorian' }">ميلادي</button>
        <button @click="switchCalendar('hijri')" :class="{ active: calendarType === 'hijri' }">هجري</button>
      </div>
  
      <div class="calendar-grid">
        <div v-for="day in days" :key="day" class="day-header">{{ day }}</div>
        <div 
          v-for="date in visibleDates"
          :key="date.date"
          class="calendar-day"
          :class="{
            'current-day': date.isCurrentDay,
            'selected-day': date.isSelected,
            'umm-alqura': date.isUmmAlQura
          }"
          @click="selectDate(date)"
        >
          {{ date.displayDate }}
          <div v-if="calendarType === 'hijri'" class="gregorian-date">
            {{ date.gregorianDate }}
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import moment from 'moment'
  import 'moment-hijri'
  
  export default {
    name: "HijriCalendar",
    props: {
      type: {
        type: String,
        default: 'gregorian' // أو 'hijri'
      }
    },
    data() {
      return {
        calendarType: this.type,
        currentDate: moment(),
        selectedDate: null
      }
    },
    computed: {
      days() {
        return this.calendarType === 'gregorian' 
          ? ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
          : ['الأحد', 'الاثنين', 'الثلاثاء', 'الأربعاء', 'الخميس', 'الجمعة', 'السبت']
      },
      visibleDates() {
        const start = this.currentDate.clone().startOf('month').startOf('week')
        const end = this.currentDate.clone().endOf('month').endOf('week')
        const dates = []
        
        let date = start.clone()
        while (date.isBefore(end)) {
          // const hijriDate = date.clone().format('iYYYY/iM/iD')
          const isUmmAlQura = this.checkUmmAlQura(date)
          
          dates.push({
            date: date.clone(),
            displayDate: this.calendarType === 'gregorian' 
              ? date.format('D') 
              : date.format('iD'),
            gregorianDate: date.format('D/M'),
            isCurrentDay: date.isSame(moment(), 'day'),
            isSelected: this.selectedDate && date.isSame(this.selectedDate, 'day'),
            isUmmAlQura: isUmmAlQura
          })
          
          date.add(1, 'day')
        }
        
        return dates
      }
    },
    methods: {
      getHeaderFormat() {
        return this.calendarType === 'gregorian' 
          ? 'MMMM YYYY' 
          : 'iMMMM iYYYY'
      },
      prevMonth() {
        this.currentDate.subtract(1, this.calendarType === 'gregorian' ? 'month' : 'iMonth')
      },
      nextMonth() {
        this.currentDate.add(1, this.calendarType === 'gregorian' ? 'month' : 'iMonth')
      },
      switchCalendar(type) {
        this.calendarType = type
        this.currentDate = moment()
      },
      selectDate(date) {
        this.selectedDate = date.date
        this.$emit('date-selected', date.date)
      },
      checkUmmAlQura(date) {
 
        const ummAlQuraDates = ['2023-07-19', '2023-07-20']  
        return ummAlQuraDates.includes(date.format('YYYY-MM-DD'))
      }
    }
  }
  </script>
  
  <style scoped>
  .calendar-container {
    max-width: 600px;
    margin: 0 auto;
    font-family: Arial, sans-serif;
  }
  
  .calendar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    background-color: #f0f0f0;
  }
  
  .calendar-switcher {
    margin: 1rem 0;
    display: flex;
    gap: 1rem;
    justify-content: center;
  }
  
  .calendar-switcher button.active {
    background-color: #4CAF50;
    color: white;
  }
  
  .calendar-grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 2px;
  }
  
  .day-header {
    padding: 1rem;
    text-align: center;
    background-color: #e0e0e0;
  }
  
  .calendar-day {
    padding: 1rem;
    text-align: center;
    background-color: #fff;
    border: 1px solid #ddd;
    cursor: pointer;
  }
  
  .current-day {
    background-color: #2196F3 !important;
    color: white;
  }
  
  .selected-day {
    background-color: #4CAF50 !important;
    color: white;
  }
  
  .umm-alqura {
    border: 2px solid #FF5722;
  }
  
  .gregorian-date {
    font-size: 0.8em;
    color: #666;
  }
  </style>