<script setup>

import { useCalendarStore } from '@/views/apps/calendar/useCalendarStore';
// 👉 Store
const store = useCalendarStore()


const checkAll = computed({

/*GET: Return boolean `true` => if length of options matches length of selected filters => Length matches when all events are selected
SET: If value is `true` => then add all available options in selected filters => Select All
Else if => all filters are selected (by checking length of both array) => Empty Selected array  => Deselect All
*/
get: () => store.selectedCalendars.length === store.availableCalendars.length,
set: val => {
  if (val)
    store.selectedCalendars = store.availableCalendars.map(i => i.label)
  else if (store.selectedCalendars.length === store.availableCalendars.length)
    store.selectedCalendars = []
},
})

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
    <VExpansionPanels
    v-model="openedPanels"
    multiple
    >
        <VExpansionPanel
        >
        <VExpansionPanelTitle class="appercase">tuteur</VExpansionPanelTitle>
            <VExpansionPanelText>
                <div class="d-flex flex-column calendars-checkbox">
                <VCheckbox
                v-for="calendar in store.availableCalendars"
                :key="calendar.label"
                v-model="store.selectedCalendars"
                :value="calendar.label"
                :color="calendar.color"
                :label="calendar.label"
                />
                </div>
            </VExpansionPanelText>
        </VExpansionPanel>
    </VExpansionPanels>
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
.v-expansion-panel-title{
font-family: 'Geomanist-Book';
background-color: #EFEFEF;
border-radius: 7px;
height: 36px !important;
letter-spacing: 0.35px;
color: #133155 !important;
text-transform: uppercase;
margin-bottom: 10px;
}
</style>
