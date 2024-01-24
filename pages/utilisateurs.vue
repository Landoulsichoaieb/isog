
<script setup>
import datattableutilisateurs from './tables/datattableutilisateurs';
</script>
<template>
  <VRow>
    <VCol
      cols="12"
      md="12"
    >
      <VCard class="border r-card table-container-gestion" >
        <VCol cols="12">
          <datattableutilisateurs />
        </VCol>
      </VCard>
    </VCol>
  </VRow>
</template>
<style lang="scss">
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
  padding:49px 10.74%;
}

</style>
