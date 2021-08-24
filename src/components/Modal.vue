<template>
  <section class="modal">
    <div class="modal__content" v-click-outside="closeModal">
      <p>
        Il faut avoir entré 5 mots à la suite dans leur temps imparti pour pouvoir passer au niveau suivant.
        La vitesse de frappe est de 30 mots par minute. <br/>
        <strong>Les mots sont en anglais.</strong>
      </p>
      <p>
        <ul>
          <li>
            A chaque nouveau niveau, 1 mot de plus à entrer s'ajoute pour pouvoir passer au niveau suivant.
          </li>
          <li>
            Il doit alors recommencer, et se retrouve au niveau 1 du début.
          </li>
        </ul>
      </p>
    </div>
  </section>
</template>

<script>
import Vue from 'vue';
import vClickOutside from 'v-click-outside'

Vue.use(vClickOutside);

export default {
  name: 'modal',

  methods: {
    closeModal() {
      this.$emit('close-modal');
    }
  }
}
</script>

<style lang="scss" scoped>
  .modal {
    display: block;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color:rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);

    &__content {
      background-color: white;
      padding: 10px;
      position: absolute;
      top:0;
      bottom: 0;
      left: 0;
      right: 0;
      height: 62%;
      width: 90%;
      margin: auto;
    }
  }
</style>