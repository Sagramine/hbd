<template>
  <!-- Background -->
  <div :class="darkMode() ? 'darkBG' : 'lightBG'">
    <!--
      Particles
      Documentation : https://vue-particles.netlify.app/
    -->
    <vue-particles
      class="particles"
      hoverMode="grab"
      clickMode="push"
      linesColor="#dedede"
      shapeType="circle"
      :color="darkMode() ? '#f5f5f5' : '#171717'"
      :particleOpacity="0.7"
      :particlesNumber="30"
      :particleSize="4"
      :linesWidth="1"
      :lineLinked="true"
      :lineOpacity="0.4"
      :linesDistance="150"
      :moveSpeed="3"
      :hoverEffect="true"
      :clickEffect="true"
    />

    <!-- Audio file -->
    <audio autoplay loop id="playAudio">
    <source src="~/assets/music.mp3">
    </audio>

    <!-- Main -->
    <div class="main">
      <!--
        Typing characters (greeting & nickname)
        Documentation : https://github.com/Orlandster/vue-typed-js/
      -->
      <!-- prettier-ignore -->
      <vue-typed-js
        class="vue-typed"
        :showCursor="false"
        :loop="true"
        :strings="[greetings() + ', ' + nicknames + '! ^5000', 'Maafkan aku jika aku pernah menyakiti perasaanmu 💕✨ ^4000', 'Semoga kamu suka dengan yang aku buat / いつも幸せ!']"
        :backDelay="500"
        :typeSpeed="120"
        :backSpeed="20"
        :contentType="'null'"
      >
        <h1 class="typing"></h1>
      </vue-typed-js>

      <!-- Quotes -->
      <h2 class="quotes quotes-main">{{ quotes.quotes }}</h2>
      <h3 class="quotes author">{{ quotes.author }}</h3>
    </div>
  </div>
</template>

<script>
// Import quotes and variables
import quotesFile from "~/assets/quotes.js";
import variables from "~/assets/variables.js";

// Define greetings
const { greetings } = variables;

// Get time
const now = new Date().getHours();

// Client-side rendering
if (process.browser) {
  /*
   * Function random polyfill
   * Usage   : random(["this", "is", "an", "array"]);
   * Output  : randomize array values
   */
  function random(arr) {
    return arr[Math.floor(Math.random() * arr.length)];
  }

  // Window on ready (server has rendered everything)
  window.onNuxtReady(app => {
    // Add event for click
    document.addEventListener("click", function(ev) {
      // Get coordinates where user clicks (X & Y)
      const x = ev.clientX;
      const y = ev.clientY;

      // Make a new element
      let el = document.createElement("div");

      // Fill with ♥
      el.innerHTML = `<h1>${random(["💖", "💕💕", "💝💝💝"])}</h1>`;

      // Add "popup" class
      el.classList.add("popup");

      // Define the position based on where user's clicked
      el.style.left = `${x}px`;
      el.style.top = `${y}px`;

      // レンダリングする (show it to user)
      document.body.appendChild(el);

      // ラグを減らすために、3秒後に削除する前にタイムアウトを設定します
      setTimeout(function() {
        el.style.display = "none";
      }, 3000);
    });
  });
}

export default {
  transition: "slide-x-transition",
  data() {
    return {
      // prettier-ignore
      nicknames: this.random(variables.nicknames), // random the nicknames (from variables.js)
      quotes: this.random(quotesFile), // random the quotes (from variables.js)
      greetings: function() {
        // 現在が18以上の場合 (6pm), 夕方の挨拶を表示
        if (now >= 18) return greetings.evening;

        // 現在が1以上の場合 (1pm), 午後の挨拶を表示
        if (now >= 16) return greetings.afternoon;

        // 現在が12以上の場合 (11am), 日の挨拶を表示
        if (now >= 12) return greetings.day;

        // 現在が5以上の場合 (5am), 朝の挨拶を表示
        if (now >= 5) return greetings.morning;

        // 現在が0以上の場合 (12am), 睡眠の挨拶を表示
        if (now >= 0) return greetings.night;
      },
      darkMode: function() {
        // If right now is equals to or greater than 18 (6pm), turn on darkmode
        if (now >= 18) return true;

        // If right now is equals to or greater than 15 (3pm), turn on daymode
        if (now >= 15) return false;

        // If right now is equals to or greater than 11 (11am), turn on daymode
        if (now >= 11) return false;

        // If right now is equals to or greater than 5 (5am), turn on daymode
        if (now >= 5) return false;

        // If right now is equals to or greater than 0 (12am), turn on darkmode
        if (now >= 0) return true;
      }
    };
  },
  methods: {
    /* server side methods */
    // Randomize array element
    random: function(arr) {
      return arr[Math.floor(Math.random() * arr.length)];
    }
  }
};
</script>

<style lang="scss">
@import "~/assets/css/animation.scss";
@import "~/assets/css/background.scss";
@import "~/assets/css/main.scss";
</style>
