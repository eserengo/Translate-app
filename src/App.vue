<script>

export default {
  name: "App",
  data() {
    return {
      from: {
        content: "",
        lang: "en",
      },
      to: {
        content: "",
        lang: "es",
      },
    }
  },
  methods: {
    async translate() {
      try {
        const res = await fetch(`https://api.mymemory.translated.net/get?q=${this.from.content}&langpair=${this.from.lang}|${this.to.lang}`);
        if (res.ok) {
          const json = await res.json();
          this.to.content = json.responseData.translatedText;
          return true;
        }
        throw new Error();
      } catch (error) {
        this.to.content = "Failed to get data";
        return false;
      }
    },

    clear() {
      this.from.content = "";
    },

    swap() {
      let tempValue = this.from.lang;
      this.from.lang = this.to.lang;
      this.to.lang = tempValue;
    },

    copyToClipboard(value) {
      navigator.clipboard.writeText(value);
    },

    textToSpeech(text, lang) {
      if ('speechSynthesis' in window) {
        let msg = new SpeechSynthesisUtterance();
        msg.text = text;
        msg.lang = lang;
        window.speechSynthesis.speak(msg);
      } else {
        alert("Sorry, your browser doesn't support text to speech!");
      }
    },
  },
  computed: {
    contentLength() {
      return this.from.content.length;
    },
  },
}
</script>

<template>
  <div class="app">
    <aside class="logo">
      <img src="../src/assets/logo.svg" alt="Translate.io logo" />
    </aside>

    <main class="main">
      <article class="article">
        <header class="header">
          <select
            name="languageFrom"
            class="select"
            v-model="from.lang"
          >
            <option value="en">English</option>
            <option value="es">Spanish</option>
            <option value="it">Italian</option>
            <option value="fr">French</option>
          </select>
          <button
            type="button"
            class="icon"
            @click="clear"
          >
            <img src="../src/assets/Close.svg" alt="Close icon"/>
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
              class="icon"
              @click="textToSpeech(this.from.content, this.from.lang)"
            >
              <img src="../src/assets/sound_max_fill.svg" alt="Sound max fill icon"/>
            </button>
            <button
              type="button"
              class="icon"
              @click="copyToClipboard(this.from.content)"
            >
              <img src="../src/assets/Copy.svg" alt="Copy to clipboard icon"/>
            </button>
          </div>
          <div>
            <button
              type="button"
              class="btn"
              @click="translate"
            >
              <img src="../src/assets/Sort_alfa.svg" alt="Sort alpha icon"/>
              Translate
            </button>
          </div>
        </footer>
      </article>

      <article class="article">
        <header class="header">
          <button
            type="button"
            class="icon"
            @click="swap"
          >
            <img src="../src/assets/Horizontal_top_left_main.svg" alt="Horizontal top left icon"/>
          </button>
          <select
            name="languageTo"
            class="select"
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
            maxlength="200"
            disabled
            v-model="to.content"
            >
          </textarea>
        </section>
        <footer class="footer">
          <div>
            <button
              type="button"
              class="icon"
              @click="textToSpeech(this.to.content, this.to.lang)"
            >
              <img src="../src/assets/Sound_max_fill.svg" alt="Sound max fill icon"/>
            </button>
            <button
              type="button"
              class="icon"
              @click="copyToClipboard(this.to.content)"
            >
              <img src="../src/assets/Copy.svg" alt="Copy to clipboard icon"/>
            </button>
          </div>
        </footer>
      </article>
    </main>
  </div>
</template>

<style lang="scss">
@import "./main.scss";

.app {
  background: $clr-black url("../src/assets/Hero_img.jpg") no-repeat top;
  background-size: contain;
  min-height: 100vh;

  .logo {
    background-color: transparent;
    text-align: center;
    padding-top: 20vh;
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
      border-radius: 0.75rem;
      padding: 1rem;
      width: 100%;

      & * {
        font-family: "DM Sans", sans-serif;
        font-size: 16px;
      }

      .header {
        padding-bottom: 1rem;
        border-bottom: 1px solid $clr-dark-gray;
        display: flex;
        flex-flow: row nowrap;
        align-items: center;
        justify-content: space-between;

        .icon {
          @include icon;
        }

        .select {
          @include select;
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

        .icon {
          @include icon;
        }

        .icon:first-of-type {
          margin-right: 1rem;
        }

        .btn {
          @include btn;
        }
      }
    }
  }
}

@media screen and (width >= 768px) {
  .app {
    .main {
      flex-direction: row;
    }
  }
}
</style>
