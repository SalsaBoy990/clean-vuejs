<template>
  <div>
    <img class="hover-opacity thumbnail-third pointer"
         src="/src/assets/images/img_nature.jpg"
         alt="Nature"
         @click="openModal">

    <div v-show="modal === true" class="clean-modal" :class="{'show': modal}" @click="closeModal">
      <button @click="closeModal" class="close-button gray-20 fs-18 topright">
          <font-awesome-icon :icon="['fas', 'times']" />
      </button>
      <div class="clean-modal-content content-960 card card-4 animate-zoom">
        <img src="/src/assets/images/img_nature_wide.jpg" alt="Nature">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageModal',
  data() {
    return {
      modal: false,
    }
  },
  mounted() {

  },
  methods: {
    openModal() {
      this.modal = true;
    },

    closeModal() {
      this.modal = false;
    }
  },

}

</script>
