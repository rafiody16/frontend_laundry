<template>
    <div>
        <div class="panel-header panel-header-sm">
      </div>
      <div class="content">
        <div class="row">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h4 class="card-title"> User</h4>
              </div>
              <div class="card-body">
                   <a class="btn btn-success btn-lg active" v-b-modal.modal_users @click="Add()">Tambah User</a>
                <div class="table-responsive">
                  <table class="table">
                    <thead class=" text-primary">
                      <th>Id User</th>
                      <th>Outlet</th>
                      <th>Nama</th>
                      <th>Username</th>
                      <th>Role</th>
                      <th>Aksi</th>
                    </thead>
                    <tbody>
                        
                      <tr v-for="usr in users" :key="usr">
                                <td>{{ usr.id }}</td>
                                <td>{{ usr.outlet.nama_outlet }}</td>
                                <td>{{ usr.nama }}</td>
                                <td>{{ usr.username }}</td>
                                <!-- <td>{{ usr.role }}</td> -->
                                <td>
                                    <b-badge variant="success"><span v-if="usr.role === 'admin'">Admin</span></b-badge>
                                    <b-badge variant="warning"><span v-if="usr.role === 'kasir'">Kasir</span></b-badge>
                                    <b-badge variant="info"><span v-if="usr.role === 'owner'">Owner</span></b-badge>
                                </td>
                                <td>
                                    <a v-b-modal.modal_users href="#" class="btn btn-info btn-sm active" @click="Edit(usr)">Ubah</a>
                                    <a href="#" class="btn btn-danger btn-sm active" @click="Delete(usr.id)">Hapus</a>
                                </td>
                            </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>

        <b-modal 
            id="modal_users" 
            ref="users" 
            title="Form users" 
            size="md" 
            @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <b-form-select class="form-control" v-model="id_outlet" :options="nama_outlet"></b-form-select>
                    <label for="id_outlet" class="col-form-label">Outlet</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="nama" placeholder="Input Nama" v-modal="nama" id="inputNama" class="form-control" type="text"/>
                    <label for="inputNama">Nama</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="username" placeholder="Input Username" v-modal="username" id="inputUsername" class="form-control" type="text"/>
                    <label for="inputUsername">Username</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="password" placeholder="Input Password" v-modal="password" id="inputPassword" class="form-control" type="password"/>
                    <label for="inputPassword">Password</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <select v-model="role" class="form-control">
                        <option value="admin">Admin</option>
                        <option value="kasir">Kasir</option>
                        <option value="owner">Owner</option>
                    </select>
                    <label for="inputRole">Role</label>
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
            nama: "",
            username:"",
            password:"",
            role:"",
            users: [],
            nama_outlet: [],
            outlet: [],
            action:"",
            fields : ["id","name","username","role","outlet","aksi"],
        }
    },
    methods: {
        getData: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

          axios.get(base_url + '/user', config)
          .then( response => {
              console.log(response);
            if(response.data.success == true){
                this.users = response.data.data.users;
                this.outlet = response.data.data.outlet;
            }
          })

        },

        OutletDropdown: function(){
    let config= {
        headers:{
            "Authorization":"Bearer "+this.$cookies.get('Authorization')
        }
    };
    axios.get(base_url + '/outlet', config)
        .then(response=>{
            let json_outlet = response.data.data.outlet;
            let list_outlet = [{
                value: "", text:"--Pilih Outlet--"
            }]
            json_outlet.forEach(element =>{
                list_outlet.push({
                    value: element.id_outlet, text: element.nama_outlet
                    })
                });
                this.nama_outlet = list_outlet
            })
        },

        Add: function(){
            this.action = "insert";
            this.id_outlet = "";
            this.id = "";
            this.nama = "";
            this.username = "";
            this.password = "";
            this.role = "";
        },
        Edit: function(item){
            this.action = "update";
            this.id_outlet = item.id_outlet;
            this.id = item.id;
            this.nama = item.nama;
            this.username = item.username;
            this.password = item.password;
            this.role = item.role;
        },
        Save: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

            let form = {
                "id_outlet": this.id_outlet,
                "nama": this.nama,
                "username": this.username,
                "password": this.password,
                "role": this.role,
            }

            //logika method post/get (insert /update)
            if(this.action == "insert"){
                axios.post(base_url + '/user', form, config)
                .then( response => {
                    Swal.fire({
                title: 'Success',
                text: 'Data Berhasil Ditambahkan!',
                icon: 'success',
                confirmButtonText: 'OK'
            })
                })
            } else { //update
                axios.put(base_url + '/user/' + this.id, form, config)
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
                
                axios.delete(base_url + '/user/' + id, config)
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
        this.OutletDropdown();
    },
}
</script>