<template>
<div>
  <div class="panel-header panel-header-sm"></div>
    <div class="content" v-b-modal.modal_paket>
      <div class="card-header">
          <h4 class="card-title"> {{nama_outlet}}</h4>
      </div>
          <div class="row" style="margin-top:50px; margin-left:20px;">
              <div class="col-xl-3 col-md-6">
                <div class="card" style="width: 18rem;">
                  <div class="card-body text-white" style="background: linear-gradient(90deg, rgba(0,0,0,1) 0%, rgba(25,0,89,1) 0%, rgba(244,28,254,1) 100%); box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);">
                    <h5 class="card-title">Member</h5>
                    <p class="card-text" style="font-size:60px">{{member.length}}</p>
                    <a href="" to="/member" class="text-white">Lebih detail <i class="fas fa-angle-double-right"></i></a>
                  </div>
                 </div>
                </div>
                <div class="col-xl-3 col-md-6">
                  <div class="card" style="width: 18rem;">
                    <div class="card-body text-white" style="background: linear-gradient(90deg, rgba(131,58,180,1) 0%, rgba(253,100,29,1) 53%, rgba(252,176,69,1) 100%); box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);">
                        <h5 class="card-title">Outlet</h5>
                        <p class="card-text" style="font-size:60px">{{outlet.length}}</p>
                        <a href="odp.php" class="text-white">Lebih detail <i class="fas fa-angle-double-right"></i></a>
                    </div>
                  </div>
                </div>
                <div class="col-xl-3 col-md-6">
                  <div class="card" style="width: 18rem;">
                    <div class="card-body text-white" style="background: linear-gradient(90deg, rgba(0,0,0,1) 0%, rgba(5,0,73,1) 0%, rgba(55,162,214,1) 70%); box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);">
                        <h5 class="card-title">Paket</h5>
                        <p class="card-text" style="font-size:60px">{{paket.length}}</p>
                        <a href="odp.php" class="text-white">Lebih detail <i class="fas fa-angle-double-right"></i></a>
                    </div>
                  </div>
                </div>
                <div class="col-xl-3 col-md-6">
                  <div class="card" style="width: 18rem;">
                    <div class="card-body text-white" style="background: linear-gradient(90deg, rgba(0,0,0,1) 0%, rgba(34,152,0,1) 0%, rgba(60,191,115,1) 70%); box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);">
                        <h5 class="card-title">Transaksi</h5>
                        <p class="card-text" style="font-size:60px">{{transaksi.length}}</p>
                        <a href="odp.php" class="text-white">Lebih detail <i class="fas fa-angle-double-right"></i></a>
                    </div>
                  </div>
                </div>
          </div>
          <br>
          <!-- <div class="row" style="margin-left: 20px;">
              <div class="col-xl-6">
                  <div class="card mb-4">
                      <div class="card-header">
                         <i class="fas fa-chart-area me-1"></i>
                            Area Chart Example
                      </div>
                      <div class="card-body"><canvas id="myAreaChart" width="{{transaksi.length}}%" height="{{transaksi.length}}"></canvas></div>
                  </div>
              </div>
          </div> -->
    </div>
</div>
</template>
<script>
module.exports = {
        data: function() {
            return {
                nama_outlet:"",
                member: [],
                outlet: [],
                paket: [],
                transaksi: []
            }
        },
        methods: {
            getDataMember: function() {
                let config = {
                    headers: {
                        "Authorization": "Bearer " + this.$cookies.get('Authorization')
                    }
                }
                axios.get(base_url + '/member', config)
                    .then(response => {
                        console.log(response);
                        if (response.data.success == true) {
                            this.member = response.data.data.member;
                        }
                    })
            },

            getDataOutlet: function() {
                let config = {
                    headers: {
                        "Authorization": "Bearer " + this.$cookies.get('Authorization')
                    }
                }
                axios.get(base_url + '/outlet', config)
                    .then(response => {
                        console.log(response);
                        if (response.data.success == true) {
                            this.outlet = response.data.data.outlet;
                        }
                    })
            },

            getDataPaket: function() {
                let config = {
                    headers: {
                        "Authorization": "Bearer " + this.$cookies.get('Authorization')
                    }
                }
                axios.get(base_url + '/paket', config)
                    .then(response => {
                        console.log(response);
                        if (response.data.success == true) {
                            this.paket = response.data.data.paket;
                        }
                    })
            },

            getDataTransaksi: function() {
                let conf = {
                    headers: {
                        "Authorization": 'Bearer ' + this.$cookies.get('Authorization')
                    }
                };
                let form = {
                    "tahun": this.tahun,
                    "bulan": this.bulan,
                    "tgl": this.tgl
                }
                axios.post(base_url + "/transaksi/report", form, conf)
                    .then(response => {
                        this.transaksi = response.data.data;
                    })
                    .catch(error => {
                        console.log(error);
                    });
            },

        },
        mounted() {
            this.getDataMember();
            this.getDataOutlet();
            this.getDataPaket();
            this.getDataTransaksi();


        }
    }
</script>