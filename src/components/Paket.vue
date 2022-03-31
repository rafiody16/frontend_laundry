<template>
    <div>
        <div class="panel-header panel-header-sm">
      </div>
      <div class="content">
        <div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h4 class="card-title"> Paket</h4>
              </div>
              <div class="card-body">
                   <a class="btn btn-success btn-lg active" v-b-modal.modal_paket @click="Add()">Tambah Paket</a>
                <div class="table-responsive">
                  <table class="table">
                    <thead class=" text-primary">
                      <th>Id Paket</th>
                      <th>Jenis</th>
                      <th>Harga</th>
                      <th>Aksi</th>
                    </thead>
                    <tbody>
                      <tr v-for="pak in paket" :key="pak">
                                <td>{{ pak.id_paket }}</td>
                                <td>{{ pak.jenis }}</td>
                                <td>{{ pak.harga }}</td>
                                <td>
                                    <a v-b-modal.modal_paket href="#" class="btn btn-info btn-sm active" @click="Edit(pak)">Ubah</a>
                                    <a href="#" class="btn btn-danger btn-sm active" @click="Delete(pak.id_paket)">Hapus</a>
                                </td>
                        </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>

        <b-modal 
            id="modal_paket" 
            ref="paket" 
            title="Form paket" 
            size="md" 
            @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="jenis" placeholder="Input Jenis" v-modal="jenis" id="inputJenis" class="form-control" type="text"/>
                    <label for="inputJenis">Jenis</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="harga" placeholder="Input Harga" v-modal="harga" id="inputHarga" class="form-control" type="text"/>
                    <label for="inputHarga">Harga</label>
                </div>
            </form>
        </b-modal>
        </div>
</template>
<script>
module.exports =  {
    data: function(){
        return {
            id_paket: "",
            jenis: "",
            harga:"",
            paket: [],
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

          axios.get(base_url + '/paket', config)
          .then( response => {
              console.log(response);
            if(response.data.success == true){
                this.paket = response.data.data.paket;
            }
          })

        },
        Add: function(){
            this.action = "insert";
            this.id_paket = "";
            this.jenis = "";
            this.harga = "";
        },
        Edit: function(item){
            this.action = "update";
            this.id_paket = item.id_paket;
            this.jenis = item.jenis;
            this.harga = item.harga;
        },
        Save: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

            let form = {
                "jenis": this.jenis,
                "harga": this.harga,
            }

            //logika method post/get (insert /update)
            if(this.action == "insert"){
                axios.post(base_url + '/paket', form, config)
                .then( response => {
                    Swal.fire({
                title: 'Success',
                text: 'Data Berhasil Ditambahkan!',
                icon: 'success',
                confirmButtonText: 'OK'
            })
                })
            } else { //update
                axios.put(base_url + '/paket/' + this.id_paket, form, config)
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
                
                axios.delete(base_url + '/paket/' + id, config)
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