<script setup>
import data from '@/views/demos/forms/tables/data-table/datatable';
import deleteicon from '@images/icon/delete.png';
import arrowDown from '@images/icon/down-filled-triangular-arrow.png';
import duplicateicon from '@images/icon/duplicate.png';
import phone from '@images/icon/phone.png';
import saveicon from '@images/icon/save.png';
import { VDataTable } from 'vuetify/labs/VDataTable';

const editDialog = ref(false)
const deleteDialog = ref(false)
const phoneimg = phone
const duplicateiconimg = duplicateicon
const deleteiconimg = deleteicon
const saveiconimg = saveicon

const defaultItem = ref({
  responsiveId: '',
  id: -1,
  avatar: '',
  fullName: '',
  post: '',
  email: '',
  city: '',
  startDate: '',
  salary: -1,
  age: '',
  experience: '',
  status: -1,
})

const editedItem = ref(defaultItem.value)
const editedIndex = ref(-1)
const userList = ref([])

const headers = [
  {
    title: 'Nom du patient',
    key: 'fullName',
    width: '167px',
  },
  {
    title: 'Date de naissance',
    key: 'birthday',
    width: '110px',
  },
  {
    title: 'RDV',
    key: 'appointment',
  },
  {
    title: 'Lieu',
    key: 'lieu',
  },
  {
    title: 'Box',
    key: 'box',
  },
  {
    title: 'Type',
    key: 'type',
  },
  {
    title: 'Etudiant',
    key: 'etudiant',
  },
  {
    title: 'Promotion',
    key: 'promotion',
  },
  {
    title: 'Tuteur',
    key: 'tuteur',
  },
  {
    title: 'Moniteur',
    key: 'moniteur',
  },
  {
    title: 'Action',
    key: 'actions',
  },
]
const editItem = item => {
  editedIndex.value = userList.value.indexOf(item)
  editedItem.value = { ...item }
  editDialog.value = true
}

const deleteItem = item => {
  editedIndex.value = userList.value.indexOf(item)
  editedItem.value = { ...item }
  deleteDialog.value = true
}

const close = () => {
  editDialog.value = false
  editedIndex.value = -1
  editedItem.value = { ...defaultItem.value }
}

const closeDelete = () => {
  deleteDialog.value = false
  editedIndex.value = -1
  editedItem.value = { ...defaultItem.value }
}

const save = () => {
  if (editedIndex.value > -1)
    Object.assign(userList.value[editedIndex.value], editedItem.value)
  else
    userList.value.push(editedItem.value)
  close()
}

const deleteItemConfirm = () => {
  userList.value.splice(editedIndex.value, 1)
  closeDelete()
}

onMounted(() => {
  userList.value = JSON.parse(JSON.stringify(data))
})
</script>

<template>
  <VDataTable
    :headers="headers"
    :items="data"
    :items-per-page="5"
    show-select
    class="vdatatable"
  >
    <!-- full name -->
    <template #item.fullName="{ item }">
      <div class="d-flex align-center">
        <div class="d-flex flex-column ms-3">
          <span class="d-block font-weight-medium text-high-emphasis text-truncate color-name">{{ item.fullName }}</span>
          <small class="phone-nbr-container">
            <img
              :src="phoneimg" 
              alt="telephone"
              class="img-fluid phone-img"
            />  
            <span class="phone-nbr">{{ item.phone }}</span>
          </small>
        </div>
      </div>
    </template>

    <!-- Actions -->
    <template #item.actions="{ item }">
      <div class="d-flex gap-1">
        <div @click="duplicateItem(item.raw)" class="image-border action-icon-border-blue">
        <img
              :src="duplicateiconimg" 
              alt="telephone"
              class="img-fluid action-icon-style"
            />
        </div>
        <div @click="deleteItem(item.raw)" class="image-border action-icon-border-red">
        <img
              :src="deleteiconimg" 
              alt="telephone"
              class="img-fluid action-icon-style"
            />
          </div>
        <div @click="editItem(item.raw)" class="image-border action-icon-border-blue">
        <img
              :src="saveiconimg" 
              alt="telephone"
              class="img-fluid action-icon-style"
            />
        </div>
      </div>
    </template>
    <!-- Lieu -->
    <template #item.lieu="{ item }">
      <div class="d-flex gap-1">
        <select @change="handleSelectChange(item.raw, 'lieu')">
          <option value="Marseille">Marseille</option>
          <option value="Paris">Paris</option>
        </select>
        <div class="row-down" @click="moveRowDown(item.raw)">
          <img :src="arrowDown" alt="arrowDown" class="arrowDown" />
        </div>
      </div>
    </template>

    <!-- Box -->
    <template #item.box="{ item }">
      <div class="d-flex gap-1">
        <select @change="handleSelectChange(item.raw, 'box')">
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>
        <div class="row-down" @click="moveRowDown(item.raw)">
          <img :src="arrowDown" alt="arrowDown" class="arrowDown" />
        </div>
      </div>
    </template>

    <!-- Type -->
    <template #item.type="{ item }">
      <div class="d-flex gap-1">
        <select @change="handleSelectChange(item.raw, 'type')">
          <option value="API">API</option>
          <option value="cc">cc</option>
        </select>
        <div class="row-down" @click="moveRowDown(item.raw)">
          <img :src="arrowDown" alt="arrowDown" class="arrowDown" />
        </div>
      </div>
    </template>
     <!-- Etudiant -->
     <template #item.etudiant="{ item }">
      <div class="d-flex gap-1">
        <select @change="handleSelectChange(item.raw, 'etudiant')">
          <option value="API">Laurent De LA MOTTE</option>
          <option value="cc">Damien LAPORTE</option>
          <option value="cc">Emma MICHELLE</option>
          <option value="cc">Laure BOULLEAU</option>
        </select>
        <div class="row-down" @click="moveRowDown(item.raw)">
          <img :src="arrowDown" alt="arrowDown" class="arrowDown" />
        </div>
      </div>
    </template>
    <template #item.tuteur="{ item }">
      <div class="d-flex gap-1">
        <select @change="handleSelectChange(item.raw, 'tuteur')">
          <option value="API">GIRENCE</option>
        </select>
        <div class="row-down" @click="moveRowDown(item.raw)">
          <img :src="arrowDown" alt="arrowDown" class="arrowDown" />
        </div>
      </div>
    </template>
    <template #item.moniteur="{ item }">
      <div class="d-flex gap-1">
        <select @change="handleSelectChange(item.raw, 'moniteur')">
          <option value="API">DUVAL</option>
        </select>
        <div class="row-down" @click="moveRowDown(item.raw)">
          <img :src="arrowDown" alt="arrowDown" class="arrowDown" />
        </div>
      </div>
    </template>
  </VDataTable>
  <button class="submit-button">AFFECTER</button>
  <!-- 👉 Edit Dialog  -->
  <VDialog
    v-model="editDialog"
    max-width="600px"
  >
    <VCard>
      <VCardTitle>
        <span class="headline">Edit Item</span>
      </VCardTitle>

      <VCardText>
        {{ editedItem?.fullName }}
        <VContainer>
          <VRow>
            <!-- fullName -->
            <VCol
              cols="12"
              sm="6"
              md="4"
            >
              <VTextField
                v-model="editedItem.fullName"
                label="User name"
              />
            </VCol>

            <!-- email -->
            <VCol
              cols="12"
              sm="6"
              md="4"
            >
              <VTextField
                v-model="editedItem.email"
                label="Email"
              />
            </VCol>

            <!-- salary -->
            <VCol
              cols="12"
              sm="6"
              md="4"
            >
              <VTextField
                v-model="editedItem.salary"
                label="Salary"
                prefix="$"
                type="number"
              />
            </VCol>

            <!-- age -->
            <VCol
              cols="12"
              sm="6"
              md="4"
            >
              <VTextField
                v-model="editedItem.age"
                label="Age"
                type="number"
              />
            </VCol>

            <!-- start date -->
            <VCol
              cols="12"
              sm="6"
              md="4"
            >
              <VTextField
                v-model="editedItem.startDate"
                label="Date"
              />
            </VCol>

            <!-- status -->
            <VCol
              cols="12"
              sm="6"
              md="4"
            >
              <AppSelect
                v-model="editedItem.status"
                :items="selectedOptions"
                item-title="text"
                item-value="value"
                label="Standard"
                variant="underlined"
                readonly
              />
            </VCol>
          </VRow>
        </VContainer>
      </VCardText>

      <VCardActions>
        <VSpacer />

        <VBtn
          color="error"
          variant="outlined"
          @click="close"
        >
          Cancel
        </VBtn>

        <VBtn
          color="success"
          variant="elevated"
          @click="save"
        >
          Save
        </VBtn>
      </VCardActions>
    </VCard>
  </VDialog>

  <!-- 👉 Delete Dialog  -->
  <VDialog
    v-model="deleteDialog"
    max-width="500px"
  >
    <VCard>
      <VCardTitle>
        Are you sure you want to delete this item?
      </VCardTitle>

      <VCardActions>
        <VSpacer />

        <VBtn
          color="error"
          variant="outlined"
          @click="closeDelete"
        >
          Cancel
        </VBtn>

        <VBtn
          color="success"
          variant="elevated"
          @click="deleteItemConfirm"
        >
          OK
        </VBtn>

        <VSpacer />
      </VCardActions>
    </VCard>
  </VDialog>
</template>
<style lang="scss">
@font-face {
  font-family: 'Geomanist-Regular';
  src: url('/fonts/Geomanist-Regular.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}
@font-face {
  font-family: 'Geomanist-Book';
  src: url('/fonts/Geomanist-Book.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}
.submit-button{
  float: right;
  width:126px;
  height:30px;
  line-height: 32px;
  background: #E5A439 0% 0% no-repeat padding-box;
  font-family: "Geomanist-Regular";
  font-size: 12px;
  color:#ffffff;
  margin-top: 22px;
  letter-spacing: 0.3px;
  text-transform: uppercase;
  border-radius: 11px;
}
.geomanist-regular{
  font-family: "Geomanist-Regular";
}
tbody{
  font-family: "Geomanist-Regular", sans-serif;
  padding: 1% !important;
  font-size: 12px;
  color: #133155;
}
select{
  font-family: "Geomanist-Regular";
  font-size: 12px;
  color: #133155;
}
.arrowDown{
  width: 9px;
  height: 9px;
  margin-top:5px;
}
.color-name{
  float: left;
  color: #133155 !important;
  font-weight: bold !important;
}
.phone-nbr-container{
  float: left;
  width: 122px !important;
}
.phone-nbr{
  float: left;
  margin-left:3px;
  letter-spacing: 0.3px;
}
.phone-img{
  float: left;
  width: 12.45px;
  height: 12.45px;
}
.action-icon-style{
  float: left;
  width: 14px;
  height: 14px;
}
.image-border{
border-radius: 50%;
padding:4px;
cursor:pointer;}
.action-icon-border-blue{
  border: solid 1px #133155;
}
.action-icon-border-red{
  border: solid 1px #E31051;
}
.vdatatable {
  --v-table-row-height: 56px;
  --v-table-header-height: 40px !important;
  color: #133155;
  th {
    font-family: "Geomanist-Book", sans-serif;
    font-size: 12px;
    font-weight: 500;
    letter-spacing: none;
    line-height: 16px;
    background-color: #133155;
    text-align: center !important;
    color: #ffffff !important;                     
    .v-data-table-header__content {
      display: flex;
      justify-content: center;
    }
    &:first-child {
      background-color: #ffffff;
      border: none !important;
    }

    &:nth-child(2) {
      border-radius: 11px 0px 0px 11px;  // Set border-radius for the second th
    }

    &:last-child {
      border-radius: 0px 11px 11px 0px;
    }
  }
  th:nth-child(4) {
    border-right: solid 1px #ffffff;
  }
  th:nth-child(n+5) {
    opacity: 0.5;
  }
  th:not(:nth-child(4)):not(:last-child) {
    position: relative;
  }
  th:not(:nth-child(4)):not(:last-child)::after {
      content: '';
      position: absolute;
      right: 0;
      top: 12.5%;
      height: 75%; /* Adjust the height of the second half */
      width: 0;
      border-right: solid 1px #ffffff;
  }
  tr:first-child td {
  
  border-top: solid 1px #dddde0;
    &:nth-child(2) {
        border-radius: 11px 0px 0px 0px;
    }
    &:last-child {
        border-radius: 0px 11px 0px 0px;
    }
  }

  tr:last-child td {

    &:nth-child(2) {
        border-radius: 0px 0px 0px 11px;
    }
    &:last-child {
        border-radius: 0px 0px 11px 0px;
    }
  }
  td{
    border-bottom:solid 1px #dddde0 ;
    &:first-child {
        border: none !important;
    }
    &:nth-child(2) {
      border-left:solid 1px #dddde0;
    }

    &:last-child {
        
      border-right:solid 1px #dddde0;
    }
  }
}
.custom-checkbox-checked{
  background-color: #133155;
}
// 👉 Datatable

.v-data-table{
  table{
    tbody{
      .v-data-table-group-header-row{
        td{
          background-color: rgb(var(--v-theme-surface));
        }
      }
    }
  }
}
</style>
