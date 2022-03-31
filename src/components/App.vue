<template>
    <div class="wrapper ">
    <div class="sidebar" data-color="green">
      <!--
        Tip 1: You can change the color of the sidebar using: data-color="blue | green | orange | red | yellow"
    -->
      <div class="logo">
        <a href="http://www.creative-tim.com" class="simple-text logo-mini">
          <img src="src/assets/img/pringles.png" alt="">
        </a>
        <a href="http://www.creative-tim.com" class="simple-text logo-normal" style="font-family: 'Comfortaa', cursive;">
          Laundry Bos
        </a>
      </div>
      <div class="sidebar-wrapper" id="sidebar-wrapper">
        <ul class="nav">
            
            <router-link class="nav-link" to="/">
          <li>
            <a href="">
              <i class="now-ui-icons design_app"></i>
              <p>Dashboard</p>
            </a>
          </li>
          </router-link>
          <router-link class="nav-link" to="/member" v-if="role !== 'owner'">
          <li>
            <a href="">
              <i class="now-ui-icons business_badge"></i>
              <p>Member</p>
            </a>
          </li>
          </router-link>
          <router-link class="nav-link" to="/paket" v-if="role === 'admin'">
          <li>
            <a href="">
              <i class="now-ui-icons ui-1_send"></i>
              <p>Paket</p>
            </a>
          </li>
          </router-link>
          <router-link class="nav-link" to="/outlet" v-if="role === 'admin'">
          <li>
            <a href="">
              <i class="now-ui-icons location_map-big"></i>
              <p>Outlet</p>
            </a>
          </li>
          </router-link>
          <router-link class="nav-link" to="/user" v-if="role === 'admin'">
          <li>
            <a href="">
              <i class="now-ui-icons users_single-02"></i>
              <p>User</p>
            </a>
          </li>
          </router-link>
          <router-link class="nav-link" to="/transaksi">
          <li>
            <a href="">
              <i class="now-ui-icons shopping_cart-simple"></i>
              <p>Transaksi</p>
            </a>
          </li>
          </router-link>
        </ul>
      </div>
    </div>
    <div class="main-panel" id="main-panel">
      <!-- Navbar -->
      <nav class="navbar navbar-expand-lg navbar-transparent  bg-primary  navbar-absolute" data-color="blue">
        <div class="container-fluid">
          <div class="navbar-wrapper" v-b-modal.modal_outlet>
            <div class="navbar-toggle">
              <button type="button" class="navbar-toggler">
                <span class="navbar-toggler-bar bar1"></span>
                <span class="navbar-toggler-bar bar2"></span>
                <span class="navbar-toggler-bar bar3"></span>
              </button>
            </div>
            <a class="navbar-brand" ><i class="now-ui-icons users_circle-08"></i> {{ username }} <br> {{ role }}</a>
          </div>
          <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navigation" aria-controls="navigation-index" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-bar navbar-kebab"></span>
            <span class="navbar-toggler-bar navbar-kebab"></span>
            <span class="navbar-toggler-bar navbar-kebab"></span>
          </button>
          <div class="collapse navbar-collapse justify-content-end" id="navigation">
            <ul class="navbar-nav">
              <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" id="navbarDropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                  <i class="now-ui-icons users_single-02"></i>
                  <p>
                    <span class="d-lg-none d-md-block">Some Actions</span>
                  </p>
                </a>
                <div class="dropdown-menu dropdown-menu-right" aria-labelledby="navbarDropdownMenuLink">
                  <a class="dropdown-item" @click="Logout" href="#!">Log Out</a>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </nav>
      <router-view></router-view>
      <!-- End Navbar -->
      <footer class="footer">
        <div class=" container-fluid ">
          <div class="copyright" id="copyright">
            &copy; <script>
              document.getElementById('copyright').appendChild(document.createTextNode(new Date().getFullYear()))
            </script>, Made with &#10084; Rafi Ody Prasetyo 2022
          </div>
        </div>
      </footer>
    </div>
  </div>
</template>

<script>
module.exports = {
    data: function(){
        return {
            username: "",
            role: "",
            id_outlet: "",
            nama_outlet:"",
            outlet:[]
        }
    },
    methods: {
        getInfo: function(){
          let config = {
            headers : {
              "Authorization" : "Bearer " + this.$cookies.get('Authorization')
            }
          }

          axios.get(base_url + '/login/check', config)
          .then( response => {
            if(response.data.success == true){
              this.username = response.data.data.username;
              this.role = response.data.data.role;
            }
          })

          axios.get(base_url + "/outlet", config)
                .then(response => {
                if(response.data.success == true){
                this.nama_outlet = response.data.data.nama_outlet;
            }
          })

          // axios.get(base_url + "/outlet/" + response.data.data.id_outlet, config)
          // .then(response1 => {
          //   if(response1.data.success == true){
          //     this.nama_outlet = response1.data.data.nama_outlet;
          //   }
          // })

        },

        

        Logout: function(){
            // this.$cookies.remove("Authorization");
            // this.componentName = "login";

            // window.location == front_url;

            // this.$route.push('login')

            swal.fire({
              title: "Are you sure you want to Log out?",
              text: "Once Logged out you can sign in again",
              type: "warning",
              showCancelButton: true,
              confirmButtonColor: '#DD6B55',
              confirmButtonText: 'Yes, I am sure!',
              cancelButtonText: "No, cancel it!",
              closeOnConfirm: false,
              closeOnCancel: true
            }).then((result) => {
              if (result.isConfirmed) {
                this.$cookies.remove("Authorization");
            this.componentName = "login";
            location.reload();
              }
              else {
                swal.fire('Canceled!', '', 'info')
              }
            })

        }
    },
    mounted() {
        this.getInfo();
    },
}
</script>