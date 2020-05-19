<template>
  <div class="wrapper">
    <section class="intro">
      <h1>Dungeons & Dragons Dice Strategy</h1>
      <p>
        Solution/website created by
        <a href="https://chrislagasse.com">Chris Lagasse</a>
      </p>
      <p>
        Answering FiveThirtyEight's weekly Riddler
        <a
          href="https://fivethirtyeight.com/features/can-you-find-the-best-dungeons-dragons-strategy/"
        >
          "Can You Find the Best Dungeons & Dragons Strategy"
        </a>
      </p>
      <blockquote>
        <p>
          With a fair 20-sided die, which situation produces the highest
          expected roll: advantage of disadvantage, disadvantage of advantage or
          rolling a single die?
        </p>
        <p>
          <em>Extra Credit</em>
          : Instead of maximizing your expected roll, suppose you need to roll N
          or better with your 20-sided die. For each value of N, is it better to
          use advantage of disadvantage, disadvantage of advantage or rolling a
          single die?
        </p>
      </blockquote>
    </section>
    <section class="details">
      <h2>Results</h2>
      <p>
        Every page refresh, or by clicking "Refresh Results", we calculate
        {{ rollsToAttempt }} dice rolls for each of the 3 possibilities.
      </p>
      <p>
        The "Average" tends to be relatively equal, but with a slight edge (11
        vs 10) appearing more frequency for "Disadvantage of Advantage"
      </p>
      <p>
        <strong>Conclusion:</strong>
        Disadvantage of Advantage is the winner. For the Extra Credit, the Mode
        result shows the most diversity for the "Single Die" roll, so it's more
        likely that the Single Die would benefit you more if you're looking for
        a specific value, or greater, of N.
      </p>
      <p style="text-align: center;">
        <button @click="reroll">Refresh Results</button>
      </p>
    </section>
    <section class="results">
      <div class="result--col">
        <h3>Advantage of Disadvantage</h3>
        <div class="average">
          <span>Average</span>
          {{ averageAdvantageOfDisadvantage }}
        </div>
        <div class="average">
          <span>Median</span>
          {{ medianAdvantageOfDisadvantage }}
        </div>
        <div class="average">
          <span>Mode</span>
          {{ modeAdvantageOfDisadvantage }}
        </div>
        <ul class="result--col_details">
          <li><strong>Individual Results</strong></li>
          <li
            v-for="(result, index) in rollsAdvantageOfDisadvantage"
            :key="index"
          >
            <span>
              <strong>{{ result.winner }}</strong>
            </span>
            <span class="breakdown">
              ({{ result.rollOne.rollOne }} or {{ result.rollOne.rollTwo }} =
              {{ result.rollOne.winner }}) vs ({{ result.rollTwo.rollOne }} or
              {{ result.rollTwo.rollTwo }} = {{ result.rollTwo.winner }})
            </span>
          </li>
        </ul>
      </div>
      <div class="result--col">
        <h3>Disadvantage of Advantage</h3>
        <div class="average">
          <span>Average</span>
          {{ averageDisadvantageOfAdvantage }}
        </div>
        <div class="average">
          <span>Median</span>
          {{ medianDisadvantageOfAdvantage }}
        </div>
        <div class="average">
          <span>Mode</span>
          {{ modeDisadvantageOfAdvantage }}
        </div>
        <ul class="result--col_details">
          <li><strong>Individual Results</strong></li>
          <li
            v-for="(result, index) in rollsDisadvantageOfAdvantage"
            :key="index"
          >
            <span>
              <strong>{{ result.winner }}</strong>
            </span>
            <span class="breakdown">
              ({{ result.rollOne.rollOne }} or {{ result.rollOne.rollTwo }} =
              {{ result.rollOne.winner }}) vs ({{ result.rollTwo.rollOne }} or
              {{ result.rollTwo.rollTwo }} = {{ result.rollTwo.winner }})
            </span>
          </li>
        </ul>
      </div>
      <div class="result--col">
        <h3>Single Die</h3>
        <div class="average">
          <span>Average</span>
          {{ averageGeneralRoll }}
        </div>
        <div class="average">
          <span>Median</span>
          {{ medianGeneralRoll }}
        </div>
        <div class="average">
          <span>Mode</span>
          {{ modeGeneralRoll }}
        </div>
        <ul class="result--col_details">
          <li><strong>Individual Results</strong></li>
          <li v-for="(result, index) in rollsGeneral" :key="index">
            <span>{{ result }}</span>
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
// Dungeons & Dragons Dice Roll Comparison | Solution to Riddler at Five Thirty Eight | Chris Lagasse
export default {
  data() {
    return {
      rollsToAttempt: 500,
      rollsAdvantageOfDisadvantage: [],
      rollsDisadvantageOfAdvantage: [],
      rollsGeneral: [],
      toggledBreakdowns: [],
    };
  },
  computed: {
    averageAdvantageOfDisadvantage() {
      if (this.rollsAdvantageOfDisadvantage.length <= 0) {
        return 0;
      }

      const sum = this.rollsAdvantageOfDisadvantage.reduce(function (
        total,
        current
      ) {
        return total + current.winner;
      },
      0);

      return Math.floor(sum / this.rollsAdvantageOfDisadvantage.length);
    },
    averageDisadvantageOfAdvantage() {
      if (this.rollsDisadvantageOfAdvantage.length <= 0) {
        return 0;
      }

      const sum = this.rollsDisadvantageOfAdvantage.reduce(function (
        total,
        current
      ) {
        return total + current.winner;
      },
      0);

      return Math.floor(sum / this.rollsDisadvantageOfAdvantage.length);
    },
    averageGeneralRoll() {
      if (this.rollsGeneral.length <= 0) {
        return 0;
      }

      const sum = this.rollsGeneral.reduce(function (total, current) {
        return total + current;
      }, 0);

      return Math.floor(sum / this.rollsGeneral.length);
    },
    medianAdvantageOfDisadvantage() {
      if (this.rollsAdvantageOfDisadvantage.length <= 0) {
        return 0;
      }

      // const isEven = this.rollsAdvantageOfDisadvantage.length % 2 === 0;
      const halfwayPoint = this.rollsAdvantageOfDisadvantage.length / 2;

      return this.rollsAdvantageOfDisadvantage[halfwayPoint].winner;
    },
    medianDisadvantageOfAdvantage() {
      if (this.rollsDisadvantageOfAdvantage.length <= 0) {
        return 0;
      }

      // const isEven = this.rollsDisadvantageOfAdvantage.length % 2 === 0;
      const halfwayPoint = this.rollsDisadvantageOfAdvantage.length / 2;

      return this.rollsDisadvantageOfAdvantage[halfwayPoint].winner;
    },
    medianGeneralRoll() {
      if (this.rollsGeneral.length <= 0) {
        return 0;
      }

      // const isEven = this.rollsGeneral.length % 2 === 0;
      const halfwayPoint = this.rollsGeneral.length / 2;

      return this.rollsGeneral[halfwayPoint];
    },
    modeAdvantageOfDisadvantage() {
      if (this.rollsAdvantageOfDisadvantage.length <= 0) {
        return 0;
      }

      const totals = this.rollsAdvantageOfDisadvantage.reduce(function (p, c) {
        if (c.winner in p) {
          p[c.winner]++;
        } else {
          p[c.winner] = 1;
        }
        return p;
      }, []);

      let highestFrequency = 0;
      let highestFrequencyRoll = 1;
      totals.forEach(function (frequency, roll) {
        if (frequency > highestFrequency) {
          highestFrequency = frequency;
          highestFrequencyRoll = roll;
        }
      });

      return highestFrequencyRoll;
    },
    modeDisadvantageOfAdvantage() {
      if (this.rollsDisadvantageOfAdvantage.length <= 0) {
        return 0;
      }

      const totals = this.rollsDisadvantageOfAdvantage.reduce(function (p, c) {
        if (p[c.winner]) {
          p[c.winner]++;
        } else {
          p[c.winner] = 1;
        }
        return p;
      }, []);

      let highestFrequency = 0;
      let highestFrequencyRoll = 1;
      totals.forEach(function (frequency, roll) {
        if (frequency > highestFrequency) {
          highestFrequency = frequency;
          highestFrequencyRoll = roll;
        }
      });

      return highestFrequencyRoll;
    },
    modeGeneralRoll() {
      if (this.rollsGeneral.length <= 0) {
        return 0;
      }

      const totals = this.rollsGeneral.reduce(function (p, c) {
        if (p[c]) {
          p[c]++;
        } else {
          p[c] = 1;
        }
        return p;
      }, []);

      let highestFrequency = 0;
      let highestFrequencyRoll = 1;
      totals.forEach(function (frequency, roll) {
        if (frequency > highestFrequency) {
          highestFrequency = frequency;
          highestFrequencyRoll = roll;
        }
      });

      return highestFrequencyRoll;
    },
  },
  created() {
    this.rollAdvantageOfDisadvantage();
    this.rollDisadvantageOfAdvantage();
    this.rollGeneral();
  },
  methods: {
    reroll: function () {
      this.rollAdvantageOfDisadvantage();
      this.rollDisadvantageOfAdvantage();
      this.rollGeneral();
    },
    getRandomNumber: (max = 20) => {
      const min = Math.ceil(1);

      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1)) + min; //The maximum is inclusive and the minimum is inclusive
    },
    rollAdvantage: function () {
      const response = {
        winner: 0,
        rollOne: this.getRandomNumber(),
        rollTwo: this.getRandomNumber(),
      };

      response.winner = response.rollOne;
      if (response.rollTwo > response.rollOne) {
        response.winner = response.rollTwo;
      }

      return response;
    },
    rollDisadvantage: function () {
      const response = {
        winner: 0,
        rollOne: this.getRandomNumber(),
        rollTwo: this.getRandomNumber(),
      };

      response.winner = response.rollOne;
      if (response.rollTwo < response.rollOne) {
        response.winner = response.rollTwo;
      }

      return response;
    },
    rollAdvantageOfDisadvantage: function () {
      this.rollsAdvantageOfDisadvantage = [];
      let rollsRemaining = this.rollsToAttempt;
      while (rollsRemaining > 0) {
        const response = {
          winner: 0,
          rollOne: this.rollDisadvantage(),
          rollTwo: this.rollDisadvantage(),
        };

        response.winner = response.rollOne.winner;
        if (response.rollTwo.winner > response.rollOne.winner) {
          response.winner = response.rollTwo.winner;
        }

        this.rollsAdvantageOfDisadvantage.push(response);

        rollsRemaining--;
      }
    },
    rollDisadvantageOfAdvantage: function () {
      this.rollsDisadvantageOfAdvantage = [];
      let rollsRemaining = this.rollsToAttempt;
      while (rollsRemaining > 0) {
        const response = {
          winner: 0,
          rollOne: this.rollAdvantage(),
          rollTwo: this.rollAdvantage(),
        };

        response.winner = response.rollOne.winner;
        if (response.rollTwo.winner < response.rollOne.winner) {
          response.winner = response.rollTwo.winner;
        }

        this.rollsDisadvantageOfAdvantage.push(response);

        rollsRemaining--;
      }
    },
    rollGeneral: function () {
      this.rollsGeneral = [];
      let rollsRemaining = this.rollsToAttempt;
      while (rollsRemaining > 0) {
        this.rollsGeneral.push(this.getRandomNumber());
        rollsRemaining--;
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,400;1,700&family=Nova+Cut&display=swap");

body {
  font-family: "Josefin Sans", sans-serif;
  color: #30475e;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: "Nova Cut", cursive;
  color: #84142d;
}

*:focus {
  outline-color: #dce92b;
}

button {
  display: inline-block;
  border: none;
  padding: 1rem 2rem;
  margin: 0;
  text-decoration: none;
  background: #84142d;
  color: #ffffff;
  font-family: sans-serif;
  font-size: 1rem;
  cursor: pointer;
  text-align: center;
  transition: background 250ms ease-in-out, transform 150ms ease;
  -webkit-appearance: none;
  -moz-appearance: none;
}

a,
a:visited {
  padding: 2px;
  display: inline-block;
  color: #84142d;
  font-weight: bold;
  cursor: pointer;
}
a:hover,
a:focus {
  background-color: #84142d;
  color: #fff;
}

blockquote {
  color: #ba6b57;
}

.intro,
.details {
  margin: 0 auto;
  width: 50vw;
  max-width: 900px;
}

.intro {
  border-bottom: 1px solid #30475e;
  margin-bottom: 20px;
}

.results {
  margin: 0 auto;
  width: 75vw;
  max-width: 1200px;

  border: 5px solid #ccc;
  background-color: #ccc;

  display: grid;
  grid-column-gap: 5px;
  grid-template-columns: 1fr 1fr 1fr;
  text-align: center;
}
.results .result--col {
  background-color: white;
  padding-bottom: 20px;
}

.results .result--col .average {
  font-size: 3em;
}
.results .result--col .average span {
  font-size: 0.4em;
  display: block;
}

.result--col_details {
  text-align: left;
  list-style-type: none;
  padding: 0;
}
.result--col_details li {
  padding: 5px 10px;
  border-bottom: 1px solid #ccc;
}
.result--col_details li:first-child {
  color: #84142d;
  border-bottom: none;
}
.result--col_details span {
  display: inline-block;
  font-family: monospace;
}
.result--col_details span.breakdown {
  font-size: 0.8em;
  padding-left: 10px;
}

@media screen and (max-width: 700px) {
  .intro,
  .details,
  .results {
    width: 95vw;
  }
}

@media screen and (max-width: 500px) {
  .results {
    grid-row-gap: 5px;
    grid-template-columns: 1fr;
  }

  .result--col p {
    display: block;
    font-size: 0.8em;
    color: #cccccc;
  }
  .result--col_details {
    height: 200px;
    overflow: scroll;
  }
}
</style>
