<template>
    <div>
        <div class="panel-header panel-header-sm">
      </div>
      <div class="content">
        <div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h4 class="card-title"> Outlet</h4>
              </div>
              <div class="card-body">
                   <a class="btn btn-success btn-lg active" v-b-modal.modal_outlet @click="Add()">Tambah Outlet</a>
                <div class="table-responsive">
                  <table class="table">
                    <thead class=" text-primary">
                      <th>Id Outlet</th>
                      <th>Nama Outlet</th>
                      <th>Aksi</th>
                    </thead>
                    <tbody>
                      <tr v-for="tlet in outlet" :key="tlet">
                                <td>{{ tlet.id_outlet }}</td>
                                <td>{{ tlet.nama_outlet }}</td>
                                <td>
                                    <a v-b-modal.modal_outlet href="#" class="btn btn-info btn-sm active" @click="Edit(tlet)">Ubah</a>
                                    <a href="#" class="btn btn-danger btn-sm active" @click="Delete(tlet.id_outlet)">Hapus</a>
                                </td>
                            </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>

        <b-modal 
            id="modal_outlet" 
            ref="modal" 
            title="Form Outlet" 
            size="md" 
            @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="nama_outlet" placeholder="Enter your first name" v-modal="nama_outlet" id="inputOutlet" class="form-control" type="text"/>
                    <label for="inputOutlet">Nama Outlet</label>
                </div>
            </form>
        </b-modal>
        </div>
</template>
<script>
module.exports =  {
    data: function(){
        return {
            id_outlet: "",
            nama_outlet: "",
            outlet: [],
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

          axios.get(base_url + '/outlet', config)
          .then( response => {
              console.log(response);
            if(response.data.success == true){
                this.outlet = response.data.data.outlet;
            }
          })

        },
        Add: function(){
            this.action = "insert";
            this.id_outlet = "";
            this.nama_outlet = "";
        },
        Edit: function(item){
            this.action = "update";
            this.id_outlet = item.id_outlet;
            this.nama_outlet = item.nama_outlet;
        },
        Save: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

            let form = {
                "nama_outlet": this.nama_outlet,
            }

            //logika method post/get (insert /update)
            if(this.action == "insert"){
                axios.post(base_url + '/outlet', form, config)
                .then( response => {
                    swal("Success!", "Data berhasil ditambahkan", "success");
                })
            } else { //update
                axios.put(base_url + '/outlet/' + this.id_outlet, form, config)
                .then( response => {
                    swal("Success!", "Data berhasil diubah", "success");
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
                
                axios.delete(base_url + '/outlet/' + id, config)
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

        },

    },
    mounted() {
        this.getData();
    },
}
</script>