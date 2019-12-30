<template>
  <div id="data-list-thumb-view" class="data-list-container">

    <add-new-data-sidebar :isSidebarActive="addNewDataSidebar" @closeSidebar="addNewDataSidebar = false" />

    <!--<vs-table ref="table" multiple v-model="selected" pagination :max-items="itemsPerPage" search :data="users">-->
    <vs-table ref="table" multiple v-model="selected" pagination :max-items="itemsPerPage" search :data="listpersonil">

      <div slot="header" class="flex flex-wrap-reverse items-center flex-grow justify-between">

        <div class="flex flex-wrap-reverse items-center">

          <!-- ADD NEW -->
          <div class="p-3 mb-4 mr-4 rounded-lg cursor-pointer flex items-center justify-between text-lg font-medium text-base text-primary border border-solid border-primary" @click="addNewDataSidebar = true">
              <feather-icon icon="PlusIcon" svgClasses="h-4 w-4" />
              <span class="ml-2 text-base text-primary">Add New</span>
          </div>
        </div>

        <!-- ITEMS PER PAGE -->
        <vs-dropdown vs-trigger-click class="cursor-pointer mb-4 mr-4">
          <div class="p-4 border border-solid border-grey-light rounded-full d-theme-dark-bg cursor-pointer flex items-center justify-between font-medium">
            <span class="mr-2">{{ currentPage * itemsPerPage - (itemsPerPage - 1) }} - {{ users.length - currentPage * itemsPerPage > 0 ? currentPage * itemsPerPage : users.length }} of {{ users.length }}</span>
            <feather-icon icon="ChevronDownIcon" svgClasses="h-4 w-4" />
          </div>
          <!-- <vs-button class="btn-drop" type="line" color="primary" icon-pack="feather" icon="icon-chevron-down"></vs-button> -->
          <vs-dropdown-menu>

            <vs-dropdown-item @click="itemsPerPage=4">
              <span>4</span>
            </vs-dropdown-item>
            <vs-dropdown-item @click="itemsPerPage=10">
              <span>10</span>
            </vs-dropdown-item>
            <vs-dropdown-item @click="itemsPerPage=15">
              <span>15</span>
            </vs-dropdown-item>
            <vs-dropdown-item @click="itemsPerPage=20">
              <span>20</span>
            </vs-dropdown-item>
          </vs-dropdown-menu>
        </vs-dropdown>
      </div>

      <template slot="thead">
        <vs-th>Image</vs-th>
        <vs-th sort-key="nama_lengkap">Name</vs-th>
        <vs-th sort-key="tempat_lahir">Tempat Lahir</vs-th>
        <vs-th sort-key="email_internet">Email</vs-th>
        <vs-th sort-key="edit">Aksi</vs-th>
      </template>

      <template slot-scope="{data}">
        <tbody>
          <vs-tr :data="tr" :key="indextr" v-for="(tr, indextr) in data">

            <vs-td class="img-container">
              <img src="https://logomaster.ai/static/media/gallery002.936afb9d.png" class="product-img" />
            </vs-td>
            <vs-td>
              <p class="product-nama_lengkap font-medium">{{ tr.nama_lengkap }}</p>
            </vs-td>
            <vs-td>
              <p class="product-tempat_lahir font-medium">{{ tr.tempat_lahir }}</p>
            </vs-td>
            <vs-td>
              <p class="product-email_internet font-medium">{{ tr.email_internet }}</p>
            </vs-td>
            <vs-td>
              <vs-button color="primary" type="flat" v-on:click="greet">Detail</vs-button>
              <!-- <script src="https://cdn.jsdelivr.net/npm/vue"> -->
            </vs-td>
          </vs-tr>
        </tbody>
      </template>
    </vs-table>
  </div>
</template>

<script>
import AddNewDataSidebar from '../ui-elements/data-list/AddNewDataPersonilSidebar.vue';
import axios from 'axios'
export default {
  components: {
    AddNewDataSidebar
  },
  data() {
    return {
      selected: [],
      data: [],
      users: [],
      listpersonil: [],
      itemsPerPage: 5,
      isMounted: false,
      addNewDataSidebar: false,
      name : 'vue',
    }
  },
  computed: {
    currentPage() {
      if(this.isMounted) {
        return this.$refs.table.currentx
      }
      return 0
    },
  },
  methods: {
    getOrderStatusColor(status) {
      if(status == 'on hold') return "warning"
      if(status == 'delivered') return "success"
      if(status == 'canceled') return "danger"
      return "primary"
    },
    getPopularityColor(num) {
      if(num > 90) return "success"
      if(num >70) return "primary"
      if(num >= 50) return "warning"
      if(num < 50) return "danger"
      return "primary"
    },
    formatData(data) {
      // formats data received from API
      let formattedData = data.map((item) => {
        const fields = item.fields
        let obj = {}
        for (const key of Object.keys(fields)) {
            obj[key] = fields[key].stringValue || fields[key].integerValue || fields[key].doubleValue;
        }
        return obj
      });
      return formattedData
    },
    async getList() {
        const backend = 'http://localhost:4001/listpersonil/'
        const response = await axios.get(backend)
        this.listpersonil = response.data
    },
    // const response = await axios.get("users").catch(function(error) {
    //   thisIns.$vs.notify({
    //     title: "Error",
    //     text: error,
    //     color: "danger",
    //     iconPack: "feather",
    //     icon: "icon-alert-circle"
    //   });
    // });
    greet: function (event) {
      // alert('Hello ' + this.name + '!')
      // cartButtonClicked(item) {
            // if(this.isInCart(item.objectID)) 
            // this.$router.push('/apps/eCommerce/checkout')
            this.$router.push('/myapp/personil')
            // else this.additemInCart(item)
        // }
      // if (event) {
      //   alert(event.target.tagName)
      // }
    },
  },
  created() {
    // const thisIns = '';
    // this.$http.get('http://localhost:4001/listpersonil/')
    //   .then(function (response) {
    //       thisIns.users = thisIns.formatData(response.data)
    //       console.log(thisIns.users)
    //   })
    //   .catch(function (error) {
    //     thisIns.$vs.notify({
    //       title:'Error',
    //       text: error,
    //       color:'danger',
    //       iconPack: 'feather',
    //       icon:'icon-alert-circle'})
    //   });
  },
  mounted() {
    this.isMounted = true;
    this.getList();
  }
}
</script>

<style lang="scss">
#data-list-thumb-view {
  .vs-con-table {

    .vs-table--header {
      display: flex;
      flex-wrap: wrap-reverse;
      margin-left: 1.5rem;
      margin-right: 1.5rem;
      > span {
        display: flex;
        flex-grow: 1;
      }

      .vs-table--search{
        padding-top: 0;

        .vs-table--search-input {
          padding: 0.9rem 2.5rem;
          font-size: 1rem;

          &+i {
            left: 1rem;
          }

          &:focus+i {
            left: 1rem;
          }
        }
      }
    }

    .vs-table {
      border-collapse: separate;
      border-spacing: 0 1.3rem;
      padding: 0 1rem;


      tr{
          box-shadow: 0 4px 20px 0 rgba(0,0,0,.05);
          td{
            padding: 10px;
            &:first-child{
              border-top-left-radius: .5rem;
              border-bottom-left-radius: .5rem;
            }
            &:last-child{
              border-top-right-radius: .5rem;
              border-bottom-right-radius: .5rem;
            }
            &.img-container {
              // width: 1rem;
              // background: #fff;

              span {
                display: flex;
                justify-content: center;
              }

              .product-img {
                height: 110px;
              }
            }
          }
          td.td-check{
            padding: 20px !important;
          }
      }
    }

    .vs-table--thead{
      th {
        padding-top: 0;
        padding-bottom: 0;

        .vs-table-text{
          text-transform: uppercase;
          font-weight: 600;
        }
      }
      th.td-check{
        padding: 0 15px !important;
      }
      tr{
        background: none;
        box-shadow: none;
      }
    }

    .vs-table--pagination {
      justify-content: center;
    }
  }
}
</style>
