<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="pink text-grey-4">
      <q-toolbar>
      <q-btn dense flat round @click="leftDrawerOpen = !leftDrawerOpen" icon="menu"/>
      <q-space></q-space>
     <q-btn flat round>
      <q-avatar>
      <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRd0k02sZLg1JwxkOFGtpsspUGTPb4DehPgPZhaMnBGlGKlnE_0-jdH5-hvRjsfNY-yaMg&usqp=CAU">
    </q-avatar>
    <q-menu>
          <q-list style="min-width: 100px">
            <q-separator />
            <q-item clickable v-close-popup>
              <q-item-section>Profile</q-item-section>
            </q-item>
            <q-separator />
            <q-item clickable v-close-popup @click="logout()">
              <q-item-section>Logout</q-item-section>
            </q-item>
          </q-list>
        </q-menu>
    </q-btn>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :breakpoint="500"
      class="text-white bg-primary"
    >
      <q-scroll-area class="fit">
      <q-list>
      <q-item class="justify-center text-center q-mt-md">
          <div>
              <q-avatar size="100px">
              <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRd0k02sZLg1JwxkOFGtpsspUGTPb4DehPgPZhaMnBGlGKlnE_0-jdH5-hvRjsfNY-yaMg&usqp=CAU">
            </q-avatar>
            <div class="text-weight-bold q-mt-md">{{ $q.localStorage.getItem('dataUser').namaLengkap }}</div>
            <div>Aplikasi Penjualan toko Key</div>
          </div>
        </q-item>

        <q-item clickable active-class="active" v-ripple exact :to="{name: 'dashboardAdmin'}">
        <q-item-section avatar>
          <q-icon name="payment"/>
        </q-item-section>
          <q-item-section>
            <q-item-label>Transaksi</q-item-label>
          </q-item-section>
        </q-item>
         <q-item clickable active-class="active" v-ripple exact :to="{name: 'dataDVD'}">
        <q-item-section avatar>
          <q-icon name="dashboard"/>
        </q-item-section>
          <q-item-section>
            <q-item-label>Data Barang</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable active-class="active" v-ripple exact :to="{name: 'inputBarang'}">
        <q-item-section avatar>
          <q-icon name="input"/>
        </q-item-section>
          <q-item-section>
            <q-item-label>input pesanan</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable active-class="active" :to="{name: 'dataUser'}" v-ripple exact>
        <q-item-section avatar>
          <q-icon name="supervisor_account"/>
        </q-item-section>
          <q-item-section>
            <q-item-label>data user</q-item-label>
          </q-item-section>
        </q-item>

      </q-list>
      </q-scroll-area >
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MainLayout',

  data () {
    return {
      leftDrawerOpen: false
    }
  },
  methods: {
    logout () {
      this.$q.localStorage.remove()
      this.$router.push({ name: 'loginPage' })
    }
  }
}
</script>
<style scoped>
.active {
  color: #000000;
  background: #d42660;
  padding-right: 10px;
}
</style>
