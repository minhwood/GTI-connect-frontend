<template>
  <div class="user-profile">
    <div class="header-container">
      <h1>Your Profile</h1>
      <div>
        <Button text="Edit profile" v-if="editable === false" class="edit-profile-btn" @click="editable = true"/>
        <Button text="Save profile" v-if="editable === true" class="save-profile-btn" @click="handleSaveButton" />
        <Button text="Cancel" v-if="editable === true" class="cancel-edit-profile-btn" @click="handleCancelButton" />
      </div>
    </div>
    <div class="profile-container">
      <profile-item :editable="editable" label="First name" :data="currentUser.first_name" v-model="currentUserForm.first_name" />
      <profile-item :editable="editable" label="Last name" :data="currentUser.last_name" v-model="currentUserForm.last_name" />
      <profile-item :editable="editable" label="Email" :data="currentUser.email" v-model="currentUserForm.email" />
      <profile-item :editable="editable" label="Phone number" :data="currentUser.profile.phone_number" v-model="currentUserForm.profile.phone_number" />      
      <profile-item :editable="editable" label="Description" :data="currentUser.profile.description" v-model="currentUserForm.profile.description" :isTextArea="true" :itemWidth=100 />
      <profile-item :editable="editable" label="Nationality" :data="currentUser.profile.nationality" v-model="currentUserForm.profile.nationality" :isSelect="true" :options="nationalities" />
      <profile-item :editable="editable" label="Sectors" :data="currentUser.profile.sectors.join(', ')" v-model="currentUserForm.profile.sectors" /> 
      <profile-item :editable="editable" label="Ocupation Title" :data="currentUser.profile.occupation_title" v-model="currentUserForm.profile.occupation_title"  />
      <profile-item :editable="editable" label="Employer" :data="currentUser.profile.employer" v-model="currentUserForm.profile.employer" />
      <profile-item :editable="editable" label="Highest education level" :data="currentUser.profile.highest_education_level" v-model="currentUserForm.profile.highest_education_level" :isSelect="true" :options="highest_edu_level" />
      <profile-item :editable="editable" label="Granted at" :data="currentUser.profile.education_grant_month_year" v-model="currentUserForm.profile.education_grant_month_year" />
      <profile-item :editable="editable" label="Academic publications" :data="currentUser.profile.academic_publicfications" v-model="currentUserForm.profile.academic_publicfications" :isTextArea="true" :itemWidth=100 />
      <profile-item :editable="editable" label="Achievements" :data="currentUser.profile.achievements" v-model="currentUserForm.profile.achievements" :isTextArea="true" :itemWidth=100 />
      <profile-item :editable="editable" label="Patents" :data="currentUser.profile.patents" v-model="currentUserForm.profile.patents" :isTextArea="true" :itemWidth=100 />
      <profile-item :editable="editable" label="Governments grants" :data="currentUser.profile.governments_grants" v-model="currentUserForm.profile.governments_grants" :isTextArea="true" :itemWidth=100 />
    </div>
  </div>
</template>

<script>
import Button from '../../../components/atoms/Button.vue'
import ProfileItem from '../../../components/atoms/ProfileItem'
import { highest_edu_level, nationalities } from '@/constants'
import store from '@/store'

export default {
  name:'UserProfile',
  components: { ProfileItem, Button },
  async mounted() {
    await store.dispatch('user/getCurrentUser').then(res => {
      this.currentUser = JSON.parse(JSON.stringify(res));
      this.currentUserForm = JSON.parse(JSON.stringify(res));
    }).catch(() => {
      return undefined
    })
  },
  setup () {
    return {}
  },
  data () {
    return {
      editable: false,
      testModel: "",
      currentUserForm: {
        profile: {
          sectors: []
        }
      },
      currentUser: {
         profile: {
          sectors: []
        }
      },
      highest_edu_level: highest_edu_level,
      nationalities: nationalities
    }
  },

  methods: {
    handleCancelButton() {
      this.editable = false
      this.currentUserForm = JSON.parse(JSON.stringify(this.currentUser))
    },
    handleSaveButton() {
      this.editable = false
      store.dispatch('user/updateCurrentUser', this.currentUserForm).then(res => {
        console.log(res)
        this.currentUserForm = JSON.parse(JSON.stringify(res));
        this.currentUser = JSON.parse(JSON.stringify(res));
      }).catch(() => {
      return undefined
    })
    }
  },
}
</script>

<style lang="scss" scoped>
.profile-container {
  padding-top: 30px;
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;
}
.user-profile h1 {
  margin: 20px 0px 20px 0px;
}

.header-container {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;
  align-items: center;
}

.edit-profile-btn {
  color: var(--primarycolour);
  background-color: var(--bgcolour);
  border: 1px solid var(--primarycolour);
}

.edit-profile-btn:hover {
  background-color: var(--primarycolour);
  color: var(--bgcolour);
}

.cancel-edit-profile-btn {
  margin-left: 10px;
  background-color: var(--bgcolour);
  border: 1px solid var(--cancelcolour);
  color: var(--cancelcolour);
}

.cancel-edit-profile-btn:hover {
  background-color: var(--cancelcolour);
  color: var(--bgcolour);
}
</style>