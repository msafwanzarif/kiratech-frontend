<template>
  <AppHeader />
  <UserBanner name="John Doe" lastOnline="2 hours ago" />
  <UsersList :users="usersToDisplay" @search="handleSearch" @select-user="selectUser" @refresh="refreshUsers()"
    @repopulate="refreshUsers(true)" :totalUsers="MAX_LENGTH" :isLoading="isLoading" @next-page="getNextPage()" />
  <UserDetailModal :user="selectedUser" @close="selectedUserEmail = ''" />
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import AppHeader from '../components/AppHeader.vue'
import UserBanner from '../components/UserBanner.vue'
import UsersList from '../components/UsersList.vue'
import UserDetailModal from '../components/UserDetailModal.vue'

const currentSeed = ref("")
const currentPage = ref(0)
const isLoading = ref(false)
const MAX_LENGTH = 63
const PAGE_SIZE = 10
const lastPage = Math.ceil(MAX_LENGTH / PAGE_SIZE)
const isLastPage = computed(() => currentPage.value === lastPage)
const resultOnLastPage = MAX_LENGTH % PAGE_SIZE || PAGE_SIZE

const usersRaw = ref<any[]>([])
const searchFilter = ref("")
// format to get { date: "14 Sep 2022", name: "Megan R. Johnson", gender: "Male", country: "Australia", email: "meganejohnson@gmail.com" },
const usersToDisplay = computed(() => {
  return usersRaw.value.map((user: Record<string, any>) => ({
    date: moment(user.registered.date).format("D MMM YYYY"),
    name: `${user.name.first} ${user.name.last}`,
    gender: user.gender.charAt(0).toUpperCase() + user.gender.slice(1),
    country: user.location.country,
    email: user.email,
    picture: user.picture.large,
    dob: moment(user.dob.date).format("D MMM YYYY"),
    city: user.location.city
  })).filter(user => searchFilter.value === "" || user.name.toLowerCase().includes(searchFilter.value.toLowerCase()) || user.email.toLowerCase().includes(searchFilter.value.toLowerCase()))
})

const listIsShort = computed(() => {
  return fetchedHandle.value && usersToDisplay.value.length < PAGE_SIZE && usersRaw.value.length < MAX_LENGTH
})

const fetchedHandle = ref(true)

watch(listIsShort, () => {
  if (listIsShort.value) {
    fetchedHandle.value = false
    getNextPage()
  }
})

const selectedUserEmail = ref("")

const selectedUser = computed(() => {
  return usersToDisplay.value.find((user: Record<string, any>) => user.email === selectedUserEmail.value) || null
})

function handleSearch(searchInput: string) {
  searchFilter.value = searchInput
}

function refreshUsers(repopulate = false) {
  if(repopulate) currentSeed.value = ""
  currentPage.value = 0
  usersRaw.value = []
  getNextPage()
}


function selectUser(email: string) {
  selectedUserEmail.value = email
}

function getNextPage() {
  if (currentPage.value == lastPage) return
  currentPage.value++
  fetchUsers()
}

async function fetchUsers() {
  if (currentPage.value > lastPage) return
  let toFetch = PAGE_SIZE
  if (isLastPage.value) toFetch = resultOnLastPage
  let linkToFetch = `https://randomuser.me/api/?page=${currentPage.value}&results=${toFetch}`
  if (currentSeed.value) {
    linkToFetch += `&seed=${currentSeed.value}`
  }
  isLoading.value = true
  fetch(linkToFetch)
    .then(response => response.json())
    .then(data => {
      isLoading.value = false
      fetchedHandle.value = true
      usersRaw.value.push(...data.results)
      if (!currentSeed.value) {
        currentSeed.value = data.info.seed
      }
    })
    .catch(error => {
      isLoading.value = false
      fetchedHandle.value = true
      console.error("Error fetching users:", error)
    })
}
</script>