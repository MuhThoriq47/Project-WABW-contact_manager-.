<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 fw-bold">Tambah Kontak</p>
        <p class="first1">Isi data diri sesuai kolom yang sudah disediakan!</p>
        <p class="peringatan">Untuk mengupload foto gunakan salinan alamat foto</p>
      </div>
    </div>
  </div>
  <div class="container mt-3">
    <div class="row">
      <div class="col-md-4">
        <form @submit.prevent="submitCreate()">
          <div class="mb-2">
            <input v-model="contact.name" type="text" class="form-control" placeholder="Nama Lengkap">
          </div>
          <div class="mb-2">
            <input v-model="contact.photo" type="text" class="form-control" placeholder="Alamat/Link Foto">
          </div>
          <div class="mb-2">
            <input v-model="contact.email" type="email" class="form-control" placeholder="Email">
          </div>
          <div class="mb-2">
            <input v-model="contact.mobile" type="number" class="form-control" placeholder="No. Telepon">
          </div>
          <div class="mb-2">
            <input v-model="contact.title" type="text" class="form-control" placeholder="Pekerjaan">
          </div>
          <div class="mb-2">
            <select v-model="contact.groupId" class="form-control" v-if="groups.length > 0">
              <option value="">Tempat Bekerja</option>
              <option :value="group.id" v-for="group of groups" :key="group.id">{{group.name}}</option>
            </select>
          </div>
          <div class="mb-2">
            <input type="submit" class="btn btn-success" value="Simpan">
          </div>
        </form>
      </div>
      <div class="col-md-4">
          <img :src="contact.photo" alt="" class="contact-img">
        </div>
    </div>
  </div>
</template>

<script>
import { ContactService } from '@/services/ContactService';
export default {
  name: "AddContact",
  data : function (){
    return {
      contact : {
        name : '',
        photo : '',
        email : '',
        mobile : '',
        title : '',
        groupId : ''
      },
      groups : []
    }
  },
  created : async function (){
    try {
      let response = await ContactService.getAllGroups();
      this.groups = response.data;
    }
    catch (error){
      console.log(error);
    }
  },
  methods : {
    submitCreate : async function (){
      try {
        let response = await ContactService.createContact(this.contact);
        if(response){
          return this.$router.push('/');
        }
        else{
          return this.$router.push('/contacts/add');
        }
      }
      catch (error){
        console.log(error);
      }
    }
  }
};
</script>

<style>
.h3 {
  color: #54B435;
  font-size: 30px;
}

.first1 {
  font-size: 20px;
}

.peringatan {
  font-style: italic;
  color: red;
}
</style>
