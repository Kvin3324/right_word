<template>
  <section>
    <div class="title">
      <h1>Word Game</h1>
    </div>
    <div class="rules" v-if="!gameStarted">
      <button type="button" class="btn btn-warning" @click="displayGame">Commencer le jeu</button>
      <button type="button" class="btn btn-primary" @click="displayModal" >Règles du jeu</button>
    </div>
    <div class="rules-modal" v-if="isModal">
      <modal
        @close-modal="closeModal">
      </modal>
    </div>
    <div class="word" v-if="gameStarted">
      <div class="word__timer">
        <p> {{ beginnerTime }} </p>
      </div>
      <div class="word__title">
        <h2>Bienvenue au niveau {{ level }}</h2>
      </div>
      <div class="word__list">
        <div class="word__list--proposition">
          <p>Mot à taper:</p>
          <p class="word--given"> {{ getFirstWord }} </p>
        </div>
        <div class="word__list__answer">
          <label>
            Votre réponse:
            <input
              type="text"
              :style="inputBackground"
              :maxlength="getFirstWordLength"
              v-model="inputAnswer"
            />
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
      randomWords: [],
      gameStarted: false,
      beginnerTime: 3,
      inputAnswer: ''
    }
  },

  created() {
    this.getWords();
  },

  computed: {
    getFirstWord() {
      return this.randomWords[0]?.word;
    },
    getFirstWordLength() {
      return this.randomWords[0]?.word.length;
    },
    inputBackground() {
      const a = 1;
      const b = .5;
      const sumAb = a + b*a;
      const wordLength = this.randomWords[0].word.length*(a + b*a) - b*a;

      return {
        background: `repeating-linear-gradient(90deg, dimgrey 0, dimgrey 1ch, transparent 0 , transparent ${sumAb}ch ) 0 100%/${wordLength}ch 2px no-repeat`
      }
    }
  },

  methods: {
    // trouver comment valider le mot: checker chaque lettre tapée
      // => checker si la length de la value de l'input === à la length du mot à trouver (pendant un @change)
    // faire method pour valider le mot si l'inputValue est good
    // supprimer le 1er mot de l'array si l'inputValue est good

    async getWords() {
      try {
        const { data } = await axios.get('https://api.datamuse.com/words?topics=Google&max=30');
        this.randomWords = data;
        console.log('words', this.randomWords);

      } catch (error) {
        console.log(error);
      }
    },
    displayGame() {
      this.gameStarted = true;
      this.timer();
    },
    displayModal() {
      this.isModal = true;
    },
    closeModal() {
      this.isModal = false;
    },
    timer() {
      const timeValue = setInterval(() => {
        // décrémenter de 1 chaque 1s
        this.beginnerTime = this.beginnerTime - 1;

        if (this.beginnerTime <= 0) {
          // casser le timer une fois à 0
          clearInterval(timeValue);
          // afficher un message "Done"
          this.beginnerTime = "Time's up !";
          // disabled l'input
        }
      }, 1000);

      // incrémenter le timer à chaque nouveau level

      return this.beginnerTime;
    }
  },

  watch: {
    'inputAnswer'(value) {
      // comparer les lettres du e.target.value avec celles du 1er mot de this.randomWords
      // si c'est bon, incrémenter la data, qui correspond a l'index du mot, de 1

      [...value].forEach((letter, i) => {
        if (value.charAt(i) === this.randomWords[0].word.charAt(i) ) {
          console.log('wesh wesh la zone c good');
        } else {
          console.log('pas good le gang');
          // si letter pas bonne, elle s'affiche en rouge
        }
      });
    }
  }
}
</script>

<style lang="scss" scoped>
section {
  .rules {
    display: flex;
    justify-content: space-around;
    margin: 20px 0;
  }

  .word {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin-top: 15%;
    margin-left: 10px;

    &__timer {
      display: flex;
      justify-content: center;
      width: 100%;
    }

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
