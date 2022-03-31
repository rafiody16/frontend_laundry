<template>
    <div>
        <div class="panel-header panel-header-sm">
      </div>
      <div class="content">
        <div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h4 class="card-title"> Member</h4>
              </div>
              <div class="card-body">
                   <a class="btn btn-success btn-lg active" v-b-modal.modal_member @click="Add()">Tambah Member</a>
                <div class="table-responsive">
                  <table class="table">
                    <thead class=" text-primary">
                      <th>Id Member</th>
                      <th>Nama</th>
                      <th>Jenis Kelamin</th>
                      <th>Telephone</th>
                      <th>Alamat</th>
                      <th>Aksi</th>
                    </thead>
                    <tbody>
                      <tr v-for="mem in member" :key="mem">
                                <td>{{ mem.id_member }}</td>
                                <td>{{ mem.nama }}</td>
                                <!-- <td>{{ mem.jenis_kelamin }}</td> -->
                                <td>
                                    <b-badge variant="warning"><span v-if="mem.jenis_kelamin === 'l'">Laki-Laki</span></b-badge>
                                    <b-badge variant="success"><span v-if="mem.jenis_kelamin === 'p'">Perempuan</span></b-badge>
                                </td>
                                <td>{{ mem.tlp }}</td>
                                <td>{{ mem.alamat }}</td>
                                <td>
                                    <a v-b-modal.modal_member href="#" class="btn btn-info btn-sm active" @click="Edit(mem)">Ubah</a>
                                    <a href="#" class="btn btn-danger btn-sm active" @click="Delete(mem.id_member)">Hapus</a>
                                </td>
                            </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>

        <b-modal 
            id="modal_member" 
            ref="modal" 
            title="Form Member" 
            size="md" 
            @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="nama" placeholder="Enter your first name" v-modal="nama" id="inputNama" class="form-control" type="text"/>
                    <label for="inputNama">Nama</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="tlp" placeholder="Enter your first name" v-modal="tlp" id="inputTlp" class="form-control" type="text"/>
                    <label for="inputTlp">Telepon</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <select v-model="jk" class="form-control">
                        <option value="l">Laki-Laki</option>
                        <option value="p">Perempuan</option>
                    </select>

                    <label for="inputJk">Jenis Kelamin</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="alamat" placeholder="Enter your first name" v-modal="alamat" id="inputAlamat" class="form-control" type="text"/>
                    <label for="inputAlamat">Alamat</label>
                </div>
            </form>
        </b-modal>
        </div>
</template>
<script>
module.exports =  {
    data: function(){
        return {
            id_member: "",
            nama: "",
            jk:"",
            tlp:"",
            alamat:"",
            member: [],
            action:""
        }
    },
    methods: {
        getData: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

          axios.get(base_url + '/member', config)
          .then( response => {
              console.log(response);
            if(response.data.success == true){
                this.member = response.data.data.member;
            }
          })

        },
        Add: function(){
            this.action = "insert";
            this.id_member = "";
            this.nama = "";
            this.jk = "";
            this.tlp = "";
            this.alamat = "";
        },
        Edit: function(item){
            this.action = "update";
            this.id_member = item.id_member;
            this.nama = item.nama;
            this.jk = item.jenis_kelamin;
            this.tlp = item.tlp;
            this.alamat = item.alamat;
        },
        Save: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

            let form = {
                "nama": this.nama,
                "alamat": this.alamat,
                "jenis_kelamin": this.jk,
                "tlp": this.tlp,
            }

            //logika method post/get (insert /update)
            if(this.action == "insert"){
                axios.post(base_url + '/member', form, config)
                .then( response => {
                    Swal.fire({
                title: 'Success',
                text: 'Data Berhasil Ditambahkan!',
                icon: 'success',
                confirmButtonText: 'OK'
            })
                })
            } else { //update
                axios.put(base_url + '/member/' + this.id_member, form, config)
                .then( response => {
                    Swal.fire({
                title: 'Success',
                text: 'Data Berhasil Diubah!',
                icon: 'success',
                confirmButtonText: 'OK'
            })
                })
            }
            
            this.getData();
            
        },
        Delete: function(id){
         swal.fire({
            title: 'Are you sure?',
            text: "You won't be able to revert this!",
            type: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes, delete it!',
            cancelButtonText: 'No, cancel!',
            buttonsStyling: true
            }).then((isConfirm) => {
              if (isConfirm.value === true) {
                let config = {
                    headers : {
                        "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                    }
                }
                
                axios.delete(base_url + '/member/' + id, config)
                .then( function(response){
                     swal.fire({
                title: 'Success',
                text: 'Data Berhasil Dihapus!',
                icon: 'success',
                confirmButtonText: 'OK'
                });
                }) 

                this.getData();
  
              }
            });

        }

    },
    mounted() {
        this.getData();
    },
}
</script>