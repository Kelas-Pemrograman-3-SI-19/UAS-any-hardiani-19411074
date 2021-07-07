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
              <div class="text-h6">Edit dvd</div>
              <div>input data dvd </div>
          </q-banner>
        </div>
        </div>
      </div>
    </div>
    <q-card flat>
      <q-card-section class="row">
      <q-form
        @submit="onSubmit"
      class="q-gutter-md col-md-6 col-xs-12"
    >
      <q-input
        filled
        v-model="form.namaBarang"
        label="nama barang"
        :rules="[ val => val && val.length > 0 || 'Mohon isi nama barang']"
      />
      <q-input
            filled
            v-model="form.harga"
            label="Harga"
            :rules="[ val => val  > 0 || 'Mohon isi Harga ']"
        />
        <q-input
            filled
            v-model="form.jumlah"
            type= "number"
            label="jumlah"
            :rules="[ val => val && val.length > 0 || 'Mohon isi jumlah pembelian']"
        />
      <q-select
       filled
        v-model="form.pilihUkuran"
         :options=" Optionpilihukuran"
          label="pilih ukuran" />

     <div class="flex">
       pilih rating
        <q-rating
      v-model="form.rating"
      size="2em"
      :max="5"
      class="q-ml-md"
      color="red"
    />
     </div>

      <q-input
      v-model="form.deskripsi"
      filled
      label="deskripsi"
      type="textarea"
    />
    <q-file accept=".jpg, image/*" color="teal" filled v-model="image" label="upload gambar">
        <template v-slot:prepend>
          <q-icon name="cloud_upload" />
        </template>
      </q-file>

      <div>
        <q-btn label="Submit" type="submit" color="primary"/>
        <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
      </div>
    </q-form>
      </q-card-section>
    </q-card>
    </q-page>
</template>
<script>
export default {
  data () {
    return {
      form: {
        namaBarang: null,
        harga: 0,
        jumlah: 0,
        pilihUkuran: null,
        rating: 0,
        deskripsi: null
      },
      OptionGenre: [
        'S',
        'M',
        'L',
        'XL',
        'XXL'
      ],
      image: null
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get(`/barang/getbyid/${this.$route.params.id}`)
        .then(res => {
          if (res.data.sukses) {
            this.form = res.data.data
          } else {
            this.$router.push({ name: 'dataDVD' })
          }
        })
    },
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.put(`barang/edit/${this.$route.params.id}`, formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.$router.push({ name: 'dataDVD' })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    }
  }
}
</script>
<style scoped>
.left{
  width: 3px;
  height: 100%;
  background-color: aquamarine;
}
</style>
