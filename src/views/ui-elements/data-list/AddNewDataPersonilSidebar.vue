<template>
  <vs-sidebar click-not-close position-right parent="body" default-index="1" color="primary" class="add-new-data-sidebar items-no-padding" spacer v-model="isSidebarActiveLocal">
    <div class="mt-6 flex items-center justify-between px-6">
        <h4>ADD NEW PERSONIL DATA</h4>
        <feather-icon icon="XIcon" @click.stop="isSidebarActiveLocal = false" class="cursor-pointer"></feather-icon>
    </div>
    <vs-divider class="mb-0"></vs-divider>

    <VuePerfectScrollbar class="scroll-area--data-list-add-new pt-4 pb-6" :settings="settings">

      <div class="p-6">
        <vs-input label="NPP"  v-model="npp" class="mt-5 w-full" />
        <vs-input label="Name"  v-model="name" class="mt-5 w-full" />
        <vs-input label="Tempat Lahir"  v-model="tempat_lahir" class="mt-5 w-full" />
        <vs-input label="Email Internet"  v-model="email_internet" class="mt-5 w-full" />

        <vs-upload text="Upload Image" class="img-upload" ref="fileUpload" />
      </div>
    </VuePerfectScrollbar>

    <div class="flex flex-wrap items-center justify-center p-6" slot="footer">
      <vs-button class="mr-6" @click="addPersonil">Add Data</vs-button>
      <vs-button type="border" color="danger" @click="isSidebarActiveLocal = false">Cancel</vs-button>
    </div>
  </vs-sidebar>
</template>

<script>
import VuePerfectScrollbar from 'vue-perfect-scrollbar'
import axios from 'axios'
export default {
  props: {
    isSidebarActive: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      npp: '',
      name: '',
      tempat_lahir: '',
      email_internet: '',
      settings: { // perfectscrollbar settings
          maxScrollbarLength: 60,
          wheelSpeed: .60,
      },
    }
  },
  computed: {
    isSidebarActiveLocal: {
      get() {
        return this.isSidebarActive
      },
      set(val) {
        if(!val) {
          this.$emit('closeSidebar');
          this.initValues();
        }
      }
    }
  },
  methods: {
    initValues() {
      this.npp = '';
      this.name = '';
      this.tempat_lahir = '';
      this.email_internet = '';
    //   this.$refs.fileUpload.srcs = [];
    },
    async addPersonil() {
        const backend='http://localhost:4001/listpersonil/'
        const data = {
            npp: this.npp,
            name: this.name,
            tempat_lahir: this.tempat_lahir,
            email_internet: this.email_internet,
        }

        const response = await axios.post(backend+'add', data)

        this.items.push({
            npp: this.npp,
            name: this.name,
            tempat_lahir: this.tempat_lahir,
            email_internet: this.email_internet,
        })

        this.npp = ''
        this.name = ''
        this.tempat_lahir = ''
        this.email_internet = ''
        //   this.getList()

    },
  },
  components: {
    VuePerfectScrollbar,
  }
}
</script>

<style lang="scss" scoped>
.add-new-data-sidebar {
  /deep/ .vs-sidebar--background {
    z-index: 52010;
  }

  /deep/ .vs-sidebar {
    z-index: 52010;
    width: 400px;
    max-width: 90vw;

    .img-upload {
      margin-top: 2rem;

      .con-img-upload {
        padding: 0;
      }

      .con-input-upload {
        width: 100%;
        margin: 0;
      }
    }
  }
}

.scroll-area--data-list-add-new {
    height: calc(100% - 4.3rem);
}
</style>
