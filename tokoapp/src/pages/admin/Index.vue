<template>
  <q-page padding>
    <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div class="col-auto">
            <div class="left blue"></div>
          </div>
          <div class="col">
            <q-banner inline-actions class="text-blue-grey-14">
              <div class="text-h6">Transaksi Data</div>
              <div>Pembelian dan transaksi</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
    <q-card class="my-card">
      <q-card-section class="bg-grey-2">
        <q-table
          :rows="data"
          flat
          :columns="columns"
          row-key="name"
        >
          <template v-slot:body="props">
            <q-tr :props="props">
              <q-td key="namaBarang" :props="props">
                {{ props.row.dataBarang[0].namaBarang }}
              </q-td>
              <q-td key="harga" :props="props">
                {{ props.row.harga }}
              </q-td>
              <q-td key="jumlah" :props="props">
                {{ props.row.jumlah }}
              </q-td>
              <q-td key="total" :props="props">
                {{ props.row.total }}
              </q-td>
              <q-td key="nama" :props="props">
                {{ props.row.dataUser[0].namaLengkap }}
              </q-td>
              <q-td key="status" :props="props">
                <q-badge v-if="props.row.status === 1" color="warning" class="q-pa-sm">
                  belum terkonfirmasi
                </q-badge>
                <q-badge v-else-if="props.row.status === 2" color="yellow" class="q-pa-sm">
                  sedang dalam pengiriman
                </q-badge>
                <q-badge v-else color="blue" class="q-pa-sm">
                  sudah diterima pembeli
                </q-badge>
              </q-td>
              <q-td key="aksi" :props="props">
                <q-btn label="Detail" @click="openDetail(props.row)" unelevated color="primary"/>
                <q-btn :disable="props.row.status !== 1 " label="konfirmasi" @click="confirm(props.row._id)" class="q-ml-sm" unelevated color="green"/>
              </q-td>
            </q-tr>
          </template>
        </q-table>
      </q-card-section>
    </q-card>
    <q-dialog
      v-model="detail"
      v-if="detail"
    >
      <q-card style="width: 700px; max-width: 80vw;">
        <q-card-section>
          <div class="text-h6">Detail order</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <div class="row">
            <div class="col">
              <div class="text-caption">
                Nama pembeli
              </div>
              <div class="text-weight-bold">
                {{ activeData.dataUser[0].namaLengkap }}
              </div>
            </div>
            <div class="col">
              <div class="text-caption">
                nama Barang
              </div>
              <div class="text-weight-bold">
                {{ activeData.dataBarang[0].namaBarang }}
              </div>
            </div>
            <div class="col">
              <div class="text-caption">
                harga
              </div>
              <div class="text-weight-bold">
                {{ activeData.harga }}
              </div>
            </div>
            <div class="col">
              <div class="text-caption">
                jumlah Pembelian
              </div>
              <div class="text-weight-bold">
                {{ activeData.jumlah }}
              </div>
            </div>
            <div class="col">
              <div class="text-caption">
                Total Harga
              </div>
              <div class="text-weight-bold">
                {{ activeData.total }}
              </div>
            </div>
          </div>
          <div class="row">
            <q-img :src="`${$baseImageURL}/${activeData.image}`"/>
          </div>
        </q-card-section>

        <q-card-actions align="right" class="bg-white text-teal">
          <q-btn flat label="OK" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      columns: [
        { name: 'namaBarang', align: 'left', label: 'nama barang', field: 'namaBarang', sortable: true },
        { name: 'harga', align: 'center', label: 'harga', field: 'harga', sortable: true },
        { name: 'jumlah', align: 'center', label: 'jumlah', field: 'jumlah', sortable: true },
        { name: 'total', align: 'center', label: 'Total', field: 'total', sortable: true },
        { name: 'nama', align: 'center', label: 'Nama', field: 'namaLengkap', sortable: true },
        { name: 'status', align: 'center', label: 'Status', field: 'status', sortable: true },
        { name: 'aksi', align: 'center', label: 'aksi', field: 'aksi' }
      ],
      data: [],
      detail: false,
      activeData: null
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('/order/getallorder')
        .then((res) => {
          if (res.data.sukses) {
            this.data = res.data.data
          }
        })
    },
    openDetail (data) {
      this.activeData = data
      this.detail = true
    },
    confirm (id) {
      this.$q.dialog({
        title: 'konfirmasi',
        message: 'Apakah anda yakin mengkonfirmasi orderan?',
        cancel: true,
        color: 'accent',
        persistent: true
      }).onOk(() => {
        this.$axios.put(`order/konfirmasiorder/${id}`)
          .then((res) => {
            if (res.data.sukses) {
              this.$showNotif(res.data.pesan, 'positive')
              this.getData()
            } else {
              this.$showNotif(res.data.pesan, 'negative')
            }
          })
      })
    }
  }
}
</script>
<style scoped>
.left{
  width: 3px;
  height: 100%;
  background-color: rgb(164, 250, 84);
}
</style>
