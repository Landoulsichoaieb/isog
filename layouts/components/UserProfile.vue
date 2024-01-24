<script setup>
import menuicon from '@/assets/images/icon/menu.png';
import { PerfectScrollbar } from 'vue3-perfect-scrollbar';

const router = useRouter()
const ability = useAbility()
const menuiconimg =  menuicon

// TODO: Get type from backend
const userData = useCookie('userData')

const { signOut } = useAuth()

async function logout() {
  try {
    await signOut({ redirect: false })

    // Remove "userData" from cookie
    userData.value = null

    // Reset user abilities
    ability.update([])
            
    navigateTo({ name: 'login' })
  }
  catch (error) {
    throw createError(error)
  }
}

const userProfileList = [
{ type: 'divider' },
  {
    type: 'navItem',
    icon: 'tabler-settings',
    title: 'Paramètres',
    to: {
      name: 'apps-user-view-id',
      params: { id: 21 },
    },
  },
  { type: 'divider' },
  {
    type: 'navItem',
    icon: 'tabler-user-cog',
    title: 'Gestion des utilisateurs',
    to: {
      name: 'apps-user-view-id',
      params: { id: 21 },
    },
  },
  { type: 'divider' },
  {
    type: 'navItem',
    icon: 'tabler-user',
    title: 'Mon compte',
    to: {
      name: 'pages-account-settings-tab',
      params: { tab: 'account' },
    },
  },
  { type: 'divider' },
  {
    type: 'navItem',
    icon: 'tabler-history',
    title: 'Historique des actions',
    to: {
      name: 'pages-account-settings-tab',
      params: { tab: 'billing-plans' },
    },
  },
  { type: 'divider' },
  {
    type: 'navItem',
    icon: 'tabler-logout',
    title: 'Déconnexion',
    onClick: logout,
  },
]
</script>

<template>
    <div class="d-flex align-center">
      <VBadge
                  v-if="userData"
                  bordered
                  location="bottom right"
                  offset-x="7"
                  offset-y="7"
                  color="success"
                  class=" d-none d-md-block"
                >
                  <VAvatar
                    class="cursor-pointer vavatar-header  d-none d-md-block"
                    :color="!(userData && userData.avatar) ? 'primary' : undefined"
                    :variant="!(userData && userData.avatar) ? 'tonal' : undefined"
                  >
                    <img
                      v-if="userData && userData.avatar"
                      :src="userData.avatar"
                      class="user-img-header "
                      alt="user-header"
                    />
                    <VIcon
                      v-else
                      icon="tabler-user"
                    />
                    </VAvatar>
                    </VBadge>
            <div class="user-header  d-none d-md-block">
              <h6 class="user-name-header">
                Ludovic GIRONCE
              </h6>
              <p class="user-role-header">
                Tuteur
              </p>
            </div>
          </div>
    <VAvatar
      class="cursor-pointer"
    >
      <img
              :src="menuiconimg" 
              alt="menu"
              class="img-fluid menu-icon"
            />  

      <!-- SECTION Menu -->
      <VMenu
        activator="parent"
        width="300"
        location="bottom end"
        offset="14px"
      >
        <VList>

          <PerfectScrollbar :options="{ wheelPropagation: false }"
          class="menu-shadow"
          >     
                <VListItem class="d-block d-md-none position-item"> 
                
                  <div class="d-flex align-center">
      <VBadge
                  v-if="userData"
                  bordered
                  location="bottom right"
                  offset-x="7"
                  offset-y="7"
                  color="success"
                >
                  <VAvatar
                    class="cursor-pointer vavatar-header"
                    :color="!(userData && userData.avatar) ? 'primary' : undefined"
                    :variant="!(userData && userData.avatar) ? 'tonal' : undefined"
                  >
                    <img
                      v-if="userData && userData.avatar"
                      :src="userData.avatar"
                      class="user-img-header"
                      alt="user-header"
                    />
                    <VIcon
                      v-else
                      icon="tabler-user"
                    />
                    </VAvatar>
                    </VBadge>
            <div class="user-header">
              <h6 class="user-name-header">
                Ludovic GIRONCE
              </h6>
              <p class="user-role-header">
                Tuteur
              </p>
            </div>
          </div>
                </VListItem>
            <template
              v-for="item in userProfileList"
              :key="item.title"
            >
              <VListItem  
                v-if="item.type === 'navItem'"
                :to="item.to"
                @click="item.onClick && item.onClick()"
              >
                <template #prepend>
                  <VIcon
                    class="me-2"
                    :icon="item.icon"
                    size="22"
                  />
                </template>
                
                <VListItemTitle>{{ item.title }}</VListItemTitle>

                <template
                  v-if="item.badgeProps"
                  #append
                >
                  <VBadge v-bind="item.badgeProps" />
                </template>
              </VListItem>

              <VDivider
                v-else
                class="my-2"
              />
            </template>
          </PerfectScrollbar>
        </VList>
      </VMenu>
      <!-- !SECTION -->
    </VAvatar>
</template>
<style lang="scss">
@font-face {
  font-family: 'Geomanist-Book';
  src: url('/fonts/Geomanist-Book.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}
.user-role{
font-family: "Geomanist-Book";
font-size: 11px;
color:#E5A439;
letter-spacing: 0.28px;
text-transform: uppercase;}
.user-name{
font-family: "Geomanist-Book";
color: #133155;
font-size: 18px;}
.margin-left-24{
  margin-left: 24px;
}
.menu-shadow {
  background: #FFFFFF 0% 0% no-repeat padding-box;
  box-shadow: 0px 3px 6px #00000029;
  border-radius: 16px;
  color: #133155;
}
.v-list-item:hover{
  color: #133155 !important;
}
.menu-icon{
  width: 30px;
  height: 30px;
}
.vavatar-header{
  width: 70px !important;
  height: 70px !important;
  border:solid 5px #FFFFFF;
  border-radius: 50%;
  box-shadow: 0px 3px 6px #00000029;

}
.user-img-header{
  width: 60px;
  height: 60px;
}
.user-header{
min-width:230px;
margin-top:12px;
margin-left:30px;}
.user-name-header{
font-family: "Geomanist-Book";
color: #133155;
font-size: 18px;
}
.user-role-header{
font-family: "Geomanist-Book";
font-size: 11px;
color:#E5A439;
letter-spacing: 0.28px;
text-transform: uppercase;
}
.dot-online{
float: left;
position: relative;
top: 28px;
left:13px;
}
.position-item{
  position: relative;
  top: 5px;
}
</style>
