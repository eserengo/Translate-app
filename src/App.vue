<script>
export default {
  name: 'App',

  data() {
    return {
      from: {
        content: '',
        lang: 'en'
      },
      to: {
        content: '',
        lang: 'es'
      }
    }
  },

  methods: {
    async translate() {
      try {
        const res = await fetch(
          `https://api.mymemory.translated.net/get?q=${this.from.content}&langpair=${this.from.lang}|${this.to.lang}`
        )
        if (res.ok) {
          const json = await res.json()
          this.to.content = json.responseData.translatedText
          return true
        }
        throw new Error()
      } catch (error) {
        this.to.content = 'Failed to get data'
        return false
      }
    },

    detectLanguage() {
      this.from.lang = navigator.language.toString().slice(0, 2)
    },

    clear() {
      this.from.content = ''
      this.to.content = ''
    },

    swap() {
      let tempValue = this.from.lang
      this.from.lang = this.to.lang
      this.to.lang = tempValue
    },

    copyToClipboard(value) {
      navigator.clipboard.writeText(value)
    },

    textToSpeech(text, lang) {
      if ('speechSynthesis' in window) {
        let msg = new SpeechSynthesisUtterance()
        msg.text = text
        msg.lang = lang
        window.speechSynthesis.speak(msg)
      } else {
        alert("Sorry, your browser doesn't support text to speech!")
      }
    }
  }
}
</script>

<template>
  <div class="translate-app">
    <aside class="logo">
      <img 
        src="../src/assets/logo.svg" 
        alt="Translate.io logo" 
        aria-hidden="true"
      />
    </aside>

    <main class="main">
      <article class="article">
        <header class="header">
          <div>
            <button 
              type="button" 
              class="ia detect" 
              @click="detectLanguage"
            >
              Detect language
            </button>
            <select 
              name="languageFrom"
              class="ia select" 
              v-model="from.lang"
            >
              <option value="en">English</option>
              <option value="es">Spanish</option>
              <option value="it">Italian</option>
              <option value="fr">French</option>
            </select>
          </div>
          <button 
            type="button" 
            aria-describedby="clear-tooltip" 
            class="ia icon" 
            @click="clear"
          >
            <img 
              src="../src/assets/Close.svg" 
              alt="Close icon"
              aria-hidden="true" 
            />
            <span 
              aria-role="tooltip" 
              id="clear-tooltip" 
              class="tooltip"
            >
              Clear all text
            </span>
          </button>
        </header>
        <section class="first section">
          <textarea
            name="contentFrom"
            class="textarea"
            autocomplete="off"
            spellcheck="false"
            autofocus="off"
            cols="50"
            rows="4"
            maxlength="200"
            placeholder="Enter text here"
            v-model="from.content"
          >
          </textarea>
          <div class="length">
            <span v-text="this.from.content.length"></span>
            <span> / 200</span>
          </div>
        </section>
        <footer class="footer">
          <div>
            <button
              type="button"
              aria-describedby="tts-from-tooltip"
              class="ia icon"
              @click="textToSpeech(this.from.content, this.from.lang)"
            >
              <img 
                src="../src/assets/sound_max_fill.svg" 
                alt="Sound max fill icon" 
                aria-hidden="true"
              />
              <span 
                aria-role="tooltip" 
                id="tts-from-tooltip" 
                class="tooltip"
              >
                Text to speech
              </span>
            </button>
            <button 
              type="button" 
              aria-describedby="ctc-from-tooltip" 
              class="ia icon" 
              @click="copyToClipboard(this.from.content)"
            >
              <img 
                src="../src/assets/Copy.svg" 
                alt="Copy to clipboard icon" 
                aria-hidden="true"
              />
              <span 
                aria-role="tooltip" 
                id="ctc-from-tooltip" 
                class="tooltip"
              >
                Copy to clipboard
              </span>
            </button>
          </div>
          <div>
            <button 
              type="button" 
              class="btn" 
              @click="translate"
            >
              <img 
                src="../src/assets/Sort_alfa.svg" 
                alt="Sort alpha icon" 
                aria-hidden="true"
              />
              Translate
            </button>
          </div>
        </footer>
      </article>

      <article class="article">
        <header class="header">
          <button 
            type="button" 
            aria-describedby="swap-tooltip" 
            class="ia icon" 
            @click="swap"
          >
            <img 
              src="../src/assets/Horizontal_top_left_main.svg" 
              alt="Horizontal top left icon" 
              aria-hidden="true"
            />
            <span 
              aria-role="tooltip" 
              id="swap-tooltip" 
              class="tooltip"
            >
              Swap languages
            </span>
          </button>
          <select 
            name="languageTo" 
            class="ia select" 
            v-model="to.lang"
          >
            <option value="en">English</option>
            <option value="es">Spanish</option>
            <option value="it">Italian</option>
            <option value="fr">French</option>
          </select>
        </header>
        <section class="second section">
          <textarea
            name="contentTo"
            class="textarea"
            autocomplete="off"
            spellcheck="false"
            autofocus="off"
            cols="50"
            rows="4"
            disabled
            v-model="to.content"
          >
          </textarea>
        </section>
        <footer class="footer">
          <div>
            <button 
              type="button" 
              aria-describedby="tts-to-tooltip" 
              class="ia icon" 
              @click="textToSpeech(this.to.content, this.to.lang)"
            >
              <img 
                src="../src/assets/Sound_max_fill.svg" 
                alt="Sound max fill icon" 
                aria-hidden="true"
              />
              <span 
                aria-role="tooltip" 
                id="tts-to-tooltip" 
                class="tooltip"
              >
                Text to speech
              </span>
            </button>
            <button 
              type="button" 
              aria-describedby="ctc-to-tooltip" 
              class="ia icon" 
              @click="copyToClipboard(this.to.content)"
            >
              <img 
                src="../src/assets/Copy.svg" 
                alt="Copy to clipboard icon" 
                aria-hidden="true"
              />
              <span 
                aria-role="tooltip" 
                id="ctc-to-tooltip" 
                class="tooltip"
              >
                Copy to clipboard
              </span>
            </button>
          </div>
        </footer>
      </article>
    </main>
  </div>
</template>

<style lang="scss">
@import './main.scss';

.translate-app {
  background: $clr-black url('../src/assets/Hero_img.jpg') no-repeat top;
  background-size: contain;
  min-height: 100vh;

  .logo {
    background-color: transparent;
    text-align: center;
    padding-top: 10vh;
  }

  .main {
    display: flex;
    flex-flow: column nowrap;
    align-items: center;
    gap: 1rem;
    padding: 1rem;

    .article {
      background-color: $clr-opaque;
      border: 2px solid $clr-dark-gray;
      box-shadow: $shadow;
      border-radius: 0.75rem;
      padding: 1rem;
      width: 100%;

      & * {
        font-family: 'DM Sans', sans-serif;
        font-size: 16px;
      }

      .header {
        padding-bottom: 1rem;
        border-bottom: 1px solid $clr-dark-gray;
        display: flex;
        flex-flow: row nowrap;
        align-items: center;
        justify-content: space-between;

        .ia {
          @include ia;
        }

        .icon {
          padding-bottom: 0.4rem;
        }

        .select {
          color: $clr-light-gray;
        }

        .detect {
          color: $clr-light-gray;
          padding: 0.665rem;
          margin-right: 0.25rem;
        }
      }

      .section {
        .textarea {
          appearance: none;
          outline: none;
          background-color: transparent;
          color: $clr-white;
          border: none;
          width: 100%;
        }

        .length {
          color: $clr-medium-gray;
          font-size: 0.75rem;
          text-align: right;
          margin-top: 0.75rem;
        }
      }

      .first.section {
        padding-block: 1rem;
      }

      .second.section {
        padding-block: 2rem;
      }

      .footer {
        padding-top: 1rem;
        border-top: 1px solid $clr-dark-gray;
        display: flex;
        flex-flow: row nowrap;
        align-items: center;
        justify-content: space-between;

        .ia {
          @include ia;
        }

        .icon {
          padding-bottom: 0.4rem;
        }

        .icon:first-of-type {
          margin-right: 0.75rem;
        }

        .btn {
          @include btn;
        }
      }
    }
  }
}

@media screen and (width >=576px) {
  .translate-app {
    .main {
      .article {
        .header {
          .detect {
            margin-right: 0.75rem;
          }
        }
      }
    }
  }
}

@media screen and (width >=768px) {
  .translate-app {
    .logo {
      padding-top: 20vh;
    }

    .main {
      flex-direction: row;

      .article {
        .header {
          .detect {
            margin-right: 0.25rem;
          }
        }
      }
    }
  }
}

@media screen and (width >=1024px) {
  .translate-app {
    .main {
      .article {
        .header {
          .detect {
            margin-right: 0.75rem;
          }
        }
      }
    }
  }
}
</style>
