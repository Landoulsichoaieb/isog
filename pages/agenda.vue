<script setup>
import frLocale from '@fullcalendar/core/locales/fr';
import FullCalendar from '@fullcalendar/vue3';
import {
blankEvent,
useCalendar,
} from './agendaComponents/calendarstore/usecalendar/useCalendar';
import { useCalendarStore } from './agendaComponents/calendarstore/usecalendar/useCalendarStore';
// Components
import CalendarEventHandler from '@/views/apps/calendar/CalendarEventHandler.vue';
import agendabox from './agendaComponents/agendabox';
import agendalieu from './agendaComponents/agendalieu';
import agendamoniteur from './agendaComponents/agendamoniteur';
import agendapromotion from './agendaComponents/agendapromotion';
import agendatuteur from './agendaComponents/agendatuteur';
import agendatypes from './agendaComponents/agendatypes';
// 👉 Store
const store = useCalendarStore()

// 👉 Event
const event = ref(structuredClone(blankEvent))
const isEventHandlerSidebarActive = ref(false)

watch(isEventHandlerSidebarActive, val => {
  if (!val)
    event.value = structuredClone(blankEvent)
})

const { isLeftSidebarOpen } = useResponsiveLeftSidebar()

// 👉 useCalendar
const { refCalendar, calendarOptions, addEvent, updateEvent, removeEvent, jumpToDate } = useCalendar(event, isEventHandlerSidebarActive, isLeftSidebarOpen)

calendarOptions.headerToolbar = {
  left: 'timeGridDay,timeGridWeek,dayGridMonth', // Change to your desired view buttons
  center: '',
  right: 'today prev,next', // Change to your desired navigation buttons
};

// Set initial view
calendarOptions.initialView = 'timeGridWeek'; // Change to your desired initial view

  // Set the French locale for FullCalendar
  calendarOptions.locales = [frLocale];

// Set buttonText for French
calendarOptions.buttonText = {
  today: 'Aujourd\'hui',
  month: 'Mois',
  week: 'Semaine',
  day: 'Jour',
};


const openedPanels = ref([0,1,2,3,4,5])

const items = ref(5)

const all = () => {
  // [...Array(5).keys()] => [0, 1, 2, 3, 4]
  openedPanels.value = [...Array(items.value).keys()]
}

const none = () => {
  openedPanels.value = []
}


</script>


<template>
  <div>
    <VCard class="border r-card table-container-gestion" >
      <!-- `z-index: 0` Allows overlapping vertical nav on calendar -->
      <VLayout style="z-index: 0;">
        <!-- 👉 Navigation drawer -->
        <VNavigationDrawer
          v-model="isLeftSidebarOpen"
          width="292"
          absolute
          touchless
          location="start"
          class="calendar-add-event-drawer"
          :temporary="$vuetify.display.mdAndDown"
        ><!--
          <div style="margin: 1.4rem;">
            <VBtn
              block
              prepend-icon="tabler-plus"
              @click="isEventHandlerSidebarActive = true"
            >
              Add event
            </VBtn>
          </div>-->
          <div class="calendar-picker-header">
              <button class="fc-icon fc-icon-chevron-left back-icon"></button>
              <span class="calendar-picker-header-title">Calendrier</span>
            </div>
          <div class="d-flex align-center justify-center pa-2 mb-3">
            
            <AppDateTimePicker
              :model-value="new Date().toJSON().slice(0, 10)"
              :config="{ inline: true }"
              class="calendar-date-picker"
              @input="jumpToDate($event.target.value)"
            />
          </div>
          <VDivider />
          <div class="pt-3">
            <p class="text-sm text-uppercase text-disabled mb-3">
                <div>
                    <agendatuteur></agendatuteur>
                    <agendamoniteur></agendamoniteur>
                    <agendatypes></agendatypes>
                    <agendabox></agendabox>
                    <agendapromotion></agendapromotion>
                    <agendalieu></agendalieu>
                </div>
            </p>
          </div>
        </VNavigationDrawer>
        <VMain>
          <VCard flat>
            <FullCalendar
              ref="refCalendar"
              :options="calendarOptions"
              :locales="locales"
            />
          </VCard>
        </VMain>
      </VLayout>
    </VCard>
    <CalendarEventHandler
      v-model:isDrawerOpen="isEventHandlerSidebarActive"
      :event="event"
      @add-event="addEvent"
      @update-event="updateEvent"
      @remove-event="removeEvent"
    />
  </div>
</template>

<style lang="scss">
@use "@core/scss/template/libs/full-calendar";

.calendars-checkbox {
  .v-label {
    color: rgba(var(--v-theme-on-surface), var(--v-high-emphasis-opacity));
    opacity: var(--v-high-emphasis-opacity);
  }
}

.calendar-add-event-drawer {
  &.v-navigation-drawer:not(.v-navigation-drawer--temporary) {
    border-end-start-radius: 0.375rem;
    border-start-start-radius: 0.375rem;
  }
}

.calendar-date-picker {
  display: none;

  +.flatpickr-input {
    +.flatpickr-calendar.inline {
      border: none;
      box-shadow: none;

      .flatpickr-months {
        border-block-end: none;
      }
    }
  }

  & ~ .flatpickr-calendar .flatpickr-weekdays {
    margin-block: 0 4px;
  }
}
</style>

<style lang="scss" scoped>
@font-face {
  font-family: 'Geomanist-Book';
  src: url('/fonts/Geomanist-Book.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}
@font-face {
  font-family: 'Geomanist-medium';
  src: url('/fonts/Geomanist-Medium.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}
.v-layout {
  overflow: visible !important;

  .v-card {
    overflow: visible;
  }
}
.r-card {
  border-radius: 22px !important;
  }
.border-card{
  border:solid 1px #707070;
}
.v-card-item {
  display: none;
}
.table-container-gestion{
  padding:17px 37px;
}
.fc-header-toolbar {
    flex-wrap: wrap;
    margin: 1.4rem;
    column-gap: 0.5rem;
    row-gap: 1rem;
  }
  .fc-toolbar-chunk {
    display: flex;
    align-items: center;

    .fc-button-group {
      .fc-button-primary {
        &,
        &:hover,
        &:not(.disabled):active {
          border-color: transparent;
          background-color: transparent;
          color: rgba(var(--v-theme-on-surface), var(--v-high-emphasis-opacity));
        }

        &:focus {
          box-shadow: none !important;
        }
      }
    }

    &:last-child {
      .fc-button-group {
        border-radius: 0.375rem;
        
        .fc-button,
        .fc-button:active {
          background-color: rgba(var(--v-theme-primary), 0.16);
          color: rgb(var(--v-theme-primary));
          font-size: 0.9375rem;
          font-weight: 500;
          letter-spacing: 0.0187rem;
          padding-inline: 1rem;
          text-transform: capitalize;

          &:not(:last-child) {
            border-inline-end: 0.0625rem solid rgba(var(--v-border-color), var(--v-border-opacity));
          }

          &.fc-button-active {
            background-color: rgba(var(--v-theme-primary), 0.24);
            color: rgb(var(--v-theme-primary));
          }
        }
      }
    }
  }
  .v-navigation-drawer--left{
    border: none;
  }
.calendar-picker-header{
  float: left;
  width: 248px;
margin-top: 28px;}
  .calendar-picker-header-title{
  float: left;
  font-family: "Geomanist-medium";
  font-size: 28px;
  margin-left: 21px;
  }
  .back-icon{
  float: left;
  width: 36px;
  height: 36px;
  border: 1px solid #E2E2E2;
  border-radius: 8px;
  color: #E5A439;
  font-size:20px;}
</style>
