<template>
  <CollectionsList
    :LeadsData="getLeadsData"
    @slideout="openSidebar"
    @deleteLead="deleteLeadData"
  />
  <div v-if="slideout" :key="render">
    <CollectionsAdd @leadsbody="postLeadData" />
  </div>
</template>
<script setup lang="ts">
const slideout = ref(false);
const render = ref(0);
const getOptions={
  method:"GET",
  headers:{
    "Content-Type": "application/json",
   Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzNlZTRlOWU1NTk2NDdjZDk4NGVmMzFhNjc4ODNkMmEiLCJkIjoiMTY4MDEwOCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMzNDkzOTB9.0V2YBlGbEMRMwE5sh1m-7WqAqfbB4_Fmqy9PX5rQoRo`,


  },
  };
  let getData=useAuthLazyFetch("https://v1-orm-gharpe.mercury.infinity-api.net/api/leads/?offset=0&limit=100&sort_column=id&sort_direction=desc",getOptions)

let getLeadsData = ref(getData.data._rawValue);
console.log("=============>",getLeadsData)
//post call
const postLeadData = async (body: Object) => {
  const options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzNlZTRlOWU1NTk2NDdjZDk4NGVmMzFhNjc4ODNkMmEiLCJkIjoiMTY4MDEwOCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMzNDkzOTB9.0V2YBlGbEMRMwE5sh1m-7WqAqfbB4_Fmqy9PX5rQoRo`,
    },
    body: JSON.stringify(body),
  };
  await useAuthLazyFetchPost(
    "https://v1-orm-gharpe.mercury.infinity-api.net/api/leads/",
    options
  );
  
  getLeadsData.value.unshift(body);
  
  slideout.value = false;
};
const renderDelete=ref(0);
const deleteLeadData = async(lead:Object) => {
  const deleteoptions={
    method:"DELETE",
    headers:{
      "Content-Type": "application/json",
      Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzNlZTRlOWU1NTk2NDdjZDk4NGVmMzFhNjc4ODNkMmEiLCJkIjoiMTY4MDEwOCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMzNDkzOTB9.0V2YBlGbEMRMwE5sh1m-7WqAqfbB4_Fmqy9PX5rQoRo`,

    },
  };
  await useAuthLazyFetchDelete(`https://v1-orm-gharpe.mercury.infinity-api.net/api/leads/${lead.uid}`,deleteoptions)
  //getLeadsData.value.shift(lead);
  renderDelete.value++;
};
const openSidebar = () => {
  slideout.value = true;
  render.value++;
};
</script>
