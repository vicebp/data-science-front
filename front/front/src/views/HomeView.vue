<template>
  <NavBar />
  <Modal/>
</template>

<script>
import { defineComponent } from 'vue';

// Components
import NavBar from '../components/NavBar.vue';
import Modal from '../components/Modal.vue';

export default defineComponent({
  name: 'HomeView',

  components: {
    NavBar,Modal,
  },
});
</script>
