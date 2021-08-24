<template>
  <section>
    <div class="title">
      <h1>Word Game</h1>
    </div>
    <div class="rules">
      <button type="button" class="btn btn-primary" @click="displayModal" >Règles du jeu</button>
    </div>
    <div class="rules-modal" v-if="isModal">
      <modal
        @close-modal="closeModal">
      </modal>
    </div>
    <div class="word">
      <div class="word__title">
        <h2>Bienvenue au niveau {{ level }}</h2>
      </div>
      <div class="word__list">
        <div class="word__list--proposition">
          <p>Mot à taper:</p>
          <p class="word--given">Hello</p>
        </div>
        <div class="word__list__answer">
          <label>
            Votre réponse:
            <input type="text" />
          </label>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Modal from './Modal.vue';
import axios from 'axios';

export default {
  components: { Modal },
  name: 'Word',

  data() {
    return {
      isModal: false,
      level: 1,
      randomWords: []
    }
  },

  created() {
    this.getWords();
  },

  methods: {
    async getWords() {
      try {
        const { data } = await axios.get('https://api.datamuse.com/words?topics=Google&max=30');
        console.log(data);
        this.randomWords = data;
        console.log('words', this.randomWords);

      } catch (error) {
        console.log(error);
      }
    },
    displayModal() {
      this.isModal = true;
    },
    closeModal() {
      this.isModal = false;
    }
  }
}
</script>

<style lang="scss" scoped>
section {
  .rules {
    margin: 20px 0;
  }

  .word {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin-top: 15%;
    margin-left: 10px;

    &__list {
      display: flex;
      flex-direction: column;
      align-items: flex-start;

      &--proposition {
        display: flex;
        align-items: center;
        margin: 20px 0;
        p {
          margin: 0;
        }
        .word--given {
          margin-left: 50px;
          font-weight: bold;
          font-size: 1.3em;
        }
      }
      &__answer {
        label {
          text-align: left;
          input {
            display: block;
            margin: 20px 0;
            border: none;
            padding: 0;
            background: repeating-linear-gradient(90deg,
              dimgrey 0, dimgrey 1ch,
              transparent 0, transparent 1ch + .5*1ch)
              0 100%/ #{7*(1ch + .5*1ch) - .5*1ch} 2px no-repeat;
            font: 5ch droid sans mono, consolas, monospace;
            letter-spacing: .5*1ch;

            &:focus {
              outline: none;
              color: dodgerblue;
            }
          }
        }
      }
    }
  }
}
</style>
