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
              <div class="text-h6">data barang</div>
              <div>katalog barang</div>
          </q-banner>
        </div>
        </div>
      </div>
    </div>
   <q-card class="my-card bg-grey">
        <q-table
      title="Treats"
      :rows="data"
      flat
      :columns="columns"
      row-key="name"
    >
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td key="namaBarang" :props="props">
            {{ props.row.namaBarang }}
          </q-td>
          <q-td key="harga" :props="props">
            Rp.  {{ props.row.harga }} ,-
          </q-td>
          <q-td key="pilihUkuran" :props="props">
              {{ props.row.pilihUkuran }}
          </q-td>
          <q-td key="jumlah" :props="props">
              {{ props.row.jumlah }}
          </q-td>
          <q-td key="deskripsi" :props="props">
            <div class="ellipsis" style="max-width:200px;">
              {{ props.row.deskripsi }}
            </div>
          </q-td>
          <q-td key="gambar" :props="props">
              {{ props.row.image }}
              <q-img
                :src="`${$baseImageURL}/${props.row.image}`"
                spinner-color="white"
                style="height: 140px; width: 150px"
                />
          </q-td>
          <q-td key="aksi" :props="props">
            <div class="row q-gutter-md">
             <q-btn :to="{ name:'formEditDVD', params: { id: props.row._id } }" label="Edit" icon="edit" color="red"/>
            <q-btn @click="deleteBarang(props.row._id)" label="Delete" icon="delete" color="negative" unelevated/>
            </div>
          </q-td>
         </q-tr>
      </template>
    </q-table>
    </q-card>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      columns: [
        { name: 'namaBarang', align: 'left', label: 'nama barang', field: 'namaBarang', sortable: true },
        { name: 'harga', align: 'left', label: 'harga', field: 'harga', sortable: true },
        { name: 'pilihUkuran', align: 'left', label: 'ukuran', field: 'pilihUkuran', sortable: true },
        { name: 'jumlah', align: 'left', label: 'jumlah', field: 'jumlah', sortable: true },
        { name: 'deskripsi', align: 'left', label: 'Description', field: 'deskripsi', sortable: true },
        { name: 'gambar', align: 'left', label: 'gambar', field: 'gambar' },
        { name: 'aksi', align: 'left', label: 'aksi', field: 'aksi' }
      ],
      data: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('barang/getall')
        .then(res => {
          if (res.data.sukses) {
            this.data = res.data.data
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    },
    deleteBarang (id) {
      this.$q.dialog({
        title: 'konfirmasi',
        message: 'Apakahanda yakin?',
        color: 'primary',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.delete(`barang/delete/${id}`)
          .then(res => {
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
  .left {
    width: 3px;
    height: 100%;
    background-color: rgb(19, 165, 19);
  }
</style>
