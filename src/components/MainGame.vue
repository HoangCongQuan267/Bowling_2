<template>
  <div class="game-div" id="game-div">
    <div class="nav-bar-div">
      <router-link to="/" class="link-btn"><b>Home</b></router-link>
      <button class="link-btn" v-on:click="reset()">Restart</button>
      <div style="padding-top: 5px">
        <b> Player: </b> abc <b> High score:</b> 0
      </div>
      <button class="link-btn" v-on:click="check_mode_function()">
        Checking Mode {{ is_check_mode ? ": On" : ": Off" }}
      </button>
      <button
        class="link-btn"
        v-if="is_check_mode"
        v-on:click="submit_function()"
      >
        Submit
      </button>
      <button
        class="link-btn"
        v-if="is_check_mode"
        v-on:click="clear_function()"
      >
        Clear
      </button>
    </div>
    <div class="score-board">
      <div class="item-2">
        <div
          class="table-label-div border-inside"
          style="background-color: brown; color: white"
        >
          <b>Frame</b>
        </div>
        <div
          class="table-label-div border-inside"
          style="background-color: brown; color: white"
        >
          <b>Elements Score</b>
        </div>
        <div
          class="table-label-div border-inside"
          style="background-color: brown; color: white"
        >
          <b>Total Score</b>
        </div>
      </div>
      <div
        class="item-1"
        v-for="(item, index) in frame_array"
        :key="item.index"
      >
        <div
          class="border-inside"
          style="background-color: brown; color: white"
          v-if="index < 9"
        >
          {{ item + 1 }}
        </div>
        <div
          class="border-inside"
          style="background-color: darkred; color: white"
          v-if="index >= 9"
        >
          {{ item + 1 }}
        </div>
        <!-- if index < 9 (frame 1-> 9) -->
        <div class="border-inside double-col" v-if="index < max_frame - 1">
          <!--is_check_mode: false  -->
          <div
            class="element-score-div"
            style="border-right: 1px solid white"
            v-if="is_check_mode === false"
          >
            {{
              first_elements_score_array[index] === "-"
                ? "&nbsp;"
                : first_elements_score_array[index]
            }}
          </div>

          <div class="element-score-div" v-if="is_check_mode === false">
            {{
              second_elements_score_array[index] === "-"
                ? "&nbsp;"
                : second_elements_score_array[index]
            }}
          </div>
          <!--is_check_mode: true  -->
          <input
            type="text"
            class="element-score-div"
            v-if="is_check_mode === true"
            onfocus="this.value=''"
            v-model.number="first_elements_score_array[index]"
            style="
              height: 1em;
              border: none;
              outline: none;
              text-align: center;
              font-size: small;
              background-color: #c1d6d2;
              border-right: 1px solid white;
            "
          />
          <input
            type="text"
            class="element-score-div"
            v-if="is_check_mode === true"
            onfocus="this.value=''"
            v-model.number="second_elements_score_array[index]"
            style="
              height: 1em;
              border: none;
              outline: none;
              text-align: center;
              font-size: small;
              background-color: #c1d6d2;
            "
          />
        </div>
        <!-- if index >= 9 (frame >=10) -->
        <div class="border-inside double-col" v-if="index >= max_frame - 1">
          <!--is_check_mode: false  -->
          <div
            class="element-score-div"
            style="border-right: 1px solid white"
            v-if="is_check_mode === false"
          >
            {{
              first_elements_score_array[index] === "-"
                ? "&nbsp;"
                : first_elements_score_array[index]
            }}
          </div>
          <div
            class="element-score-div"
            style="border-right: 1px solid white"
            v-if="is_check_mode === false"
          >
            {{
              second_elements_score_array[index] === "-"
                ? "&nbsp;"
                : second_elements_score_array[index]
            }}
          </div>
          <div class="element-score-div" v-if="is_check_mode === false">
            {{
              first_elements_score_array[index + 1] === "-"
                ? "&nbsp;"
                : first_elements_score_array[index + 1]
            }}
          </div>
          <!--is_check_mode: true  -->
          <input
            type="text"
            class="element-score-div"
            v-if="is_check_mode === true"
            onfocus="this.value=''"
            v-model.number="first_elements_score_array[index]"
            style="
              height: 1em;
              border: none;
              outline: none;
              text-align: center;
              font-size: small;
              background-color: #c1d6d2;
              border-right: 1px solid white;
            "
          />
          <input
            type="text"
            class="element-score-div"
            v-if="is_check_mode === true"
            onfocus="this.value=''"
            v-model.number="second_elements_score_array[index]"
            style="
              height: 1em;
              border: none;
              outline: none;
              text-align: center;
              font-size: small;
              background-color: #c1d6d2;
              border-right: 1px solid white;
            "
          />
          <input
            type="text"
            class="element-score-div"
            v-if="is_check_mode === true"
            onfocus="this.value=''"
            v-model.number="first_elements_score_array[index + 1]"
            style="
              height: 1em;
              border: none;
              outline: none;
              text-align: center;
              font-size: small;
              background-color: #c1d6d2;
            "
          />
        </div>
        <div class="border-inside">
          {{
            total_score_array[index] === "-"
              ? "&nbsp;"
              : total_score_array[index]
          }}
        </div>
      </div>
    </div>
    <div class="ball-trench"></div>
    <div class="the-bowling-state">
      <span>
        <div class="pin-place-div"></div>
      </span>
    </div>
    <div class="front-floor-div"></div>
    <div class="middle-floor-div"></div>
    <div class="bowling-basket-div"></div>
    <div class="bowling-basket border-inside"></div>
    <div class="pins-hole-div"></div>
    <!-- Pins -->
    <img
      src="../assets/pin.png"
      v-for="(item, index) in pins_array"
      v-bind:id="'pin-' + (item + 1)"
      :key="index"
      :alt="'Pin-' + (item + 1)"
      :class="'pin-div-' + (item + 1) + ' enable-select'"
    />
    <!-- ---- -->
    <div class="back-wall-div"></div>

    <div class="ball-track" id="ball-track"></div>
    <img
      src="../assets/ball.png"
      alt="Red-ball"
      id="red-ball"
      class="the-red-ball"
      v-on:click="choose_the_ball('red')"
    />

    <img
      src="../assets/black-ball.png"
      alt="black-ball"
      id="black-ball"
      class="the-black-ball"
      v-on:click="choose_the_ball('black')"
    />
    <img
      src="../assets/black-ball.png"
      alt="enter-the-black-ball"
      class="enter-the-black-ball throw-the-ball-animation"
      id="enter-the-black-ball"
    />
    <img
      src="../assets/ball.png"
      alt="enter-the-red-ball"
      class="enter-the-red-ball throw-the-ball-animation"
      id="enter-the-red-ball"
    />
    <img
      src="../assets/ball.png"
      alt="spining-ball"
      class="spining-ball"
      id="spining-ball"
    />
    <div class="shadow-of-the-basket-div"></div>
    <button
      id="enter-btn"
      class="button-30"
      v-on:click="throw_the_ball(type_of_the_ball)"
      @keyup.enter="throw_the_ball(type_of_the_ball)"
    >
      <b>E</b>
    </button>
    <div v-if="is_end === true" class="game-over-div" id="game-over-div">
      <h4><b>Score</b></h4>
      <h4>
        <b>{{ total_score_array[9] }}</b>
      </h4>
      <h4>{{ is_end }}</h4>
      <span
        ><button class="link-btn" v-on:click="reset()">Restart</button></span
      >
    </div>
    <div
      v-if="is_check_mode === true"
      class="check-mode-div"
      id="check-mode-div"
    ></div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import sound from "../assets/sound.mp3";
import black_ball from "../assets/black-ball.png";
import red_ball from "../assets/ball.png";
var my_track = new Audio(sound);
// outside of the component:
function initialState() {
  return {
    title: "The Bowling Game",
    max_frame: 10,
    current_frame: 0,
    ball_index: 1,
    type_of_the_ball: "",
    frame_array: Array.from(Array(10).keys()),
    first_elements_score_array: Array(11).fill("-"),
    second_elements_score_array: Array(10).fill("-"),
    total_score_array: Array(10).fill("-"),
    spare_bonus_array: Array(10).fill(0),
    is_selected: false,
    total: 0,
    is_end: false,
    is_check_mode: false,
    was_thrown: false,
    current_pins: 10,
    pins_fall_array: Array(10).fill(false),
    pins_array: Array.from(Array(10).keys()),
    pin_7: [7],
    pin_10: [10],
    pin_4: [4, 7, 8],
    pin_6: [6, 10, 9],
    pin_2_9: [2, 9, 5, 6, 8, 10],
    pin_3_8: [3, 8, 4, 5, 7, 9],
    pin_1_5: [0,1,2,3,4,5,7,8,6,9],
    random_1_5: [8, 9, 10],
    random_2_9_3_8: [4, 5, 6],
    random_4_6: [2, 3],
    score: "-",
    count_ball: 0,
  };
}
export default {
  components: {
    draggable,
  },
  name: "Data",
  data() {
    return initialState();
  },
  methods: {
    /*---------------------------------------------------------------------------------------------------*/
    reset() {
      Object.assign(this.$data, initialState());
      document.getElementById("ball-track").style.display = "none";
      document.getElementById("enter-the-red-ball").style.display = "none";
      document.getElementById("enter-the-black-ball").style.display = "none";
      document.getElementById("red-ball").style.display = "block";
      document.getElementById("black-ball").style.display = "block";
      document.getElementById("spining-ball").style.display = "none";
      for (var i = 1; i < 11; i++) {
        document.getElementById("pin-" + i).className = "pin-div-" + i;
      }
      this.current_pins = 10;
    },
    check_mode_function() {
      if (!this.is_check_mode) {
        this.reset();
        this.is_check_mode = true;
      } else {
        this.reset();
      }
    },
    clear_function() {
      this.current_frame = 0;
      this.total_score_array = Array(10).fill("-");
      this.spare_bonus_array = Array(10).fill(0);
      this.first_elements_score_array = Array(11).fill("-");
      this.second_elements_score_array = Array(10).fill("-");
      this.is_end = false;
    },
    /*---------------------------------------------------------------------------------------------------*/
    submit_function() {
      this.current_frame = 0;
      this.total_score_array = Array(10).fill("-");
      this.spare_bonus_array = Array(10).fill(0);
      while (this.total_score_array[this.max_frame - 1] === "-") {
        this.score_calculate();
        this.is_end = true;
      }
    },
    /*---------------------------------------------------------------------------------------------------*/
    choose_the_ball(type) {
      if (this.was_thrown === false) {
        document.getElementById("ball-track").style.display = "block";
        if (type === "black") {
          this.type_of_the_ball = "black";
          document.getElementById("black-ball").style.display = "none";
          document.getElementById("red-ball").style.display = "block";
          document.getElementById("enter-the-red-ball").style.display = "none";
          document.getElementById("enter-the-black-ball").style.display =
            "block";
        } else {
          this.type_of_the_ball = "red";
          document.getElementById("red-ball").style.display = "none";
          document.getElementById("black-ball").style.display = "block";
          document.getElementById("enter-the-black-ball").style.display =
            "none";
          document.getElementById("enter-the-red-ball").style.display = "block";
        }
        this.is_selected = true;
      }
    },
    /*---------------------------------------------------------------------------------------------------*/
    throw_the_ball(type) {
      if (this.is_selected) {
        this.was_thrown = true;
        this.is_selected = false;
        var game_rect = document
          .getElementById("game-div")
          .getBoundingClientRect();

        if (this.type_of_the_ball === "red") {
          var red_ball_rect = document
            .getElementById("enter-the-red-ball")
            .getBoundingClientRect();
          var ball_offset = red_ball_rect.left - game_rect.left;
        } else {
          var black_ball_rect = document
            .getElementById("enter-the-black-ball")
            .getBoundingClientRect();
          var ball_offset = black_ball_rect.left - game_rect.left;
        }
        //Audio
        my_track.play();
        //
        document.getElementById("ball-track").style.display = "none";
        document.getElementById("spining-ball").className = "spining-ball";
        document.getElementById("spining-ball").style.display = "block";
        //caculate the number of pins were fall
        this.caculate_pins_fall_function(ball_offset);
        //
        window.requestAnimationFrame(function (time) {
          window.requestAnimationFrame(function (time) {
            if (type === "red") {
              document.getElementById("spining-ball").src = red_ball;
              document.getElementById("spining-ball").style.left =
                red_ball_rect.left - game_rect.left + "px";
              document.getElementById("enter-the-red-ball").style.display =
                "none";
              document.getElementById("spining-ball").className =
                "spining-ball animation-red-ball";
            } else if (type === "black") {
              document.getElementById("spining-ball").src = black_ball;
              document.getElementById("spining-ball").style.left =
                black_ball_rect.left - game_rect.left + "px";
              document.getElementById("enter-the-black-ball").style.display =
                "none";
              document.getElementById("spining-ball").className =
                "spining-ball animation-black-ball";
            }
          });
        });
        /*----------------------------------------get elements score-----------------------------------------*/
        this.type_of_the_ball = type;
        var a = setTimeout(this.score_calculate ,3500);
      }
    },
    /*---------------------------------------------------------------------------------------------------*/
    score_calculate(){
        /*---------------------------------------------------------------------------------------------------*/
        if (this.ball_index === 1 && !this.is_check_mode) {
          this.$set(
            this.first_elements_score_array,
            this.current_frame,
            this.score
          );
        } else if (this.ball_index === 2 && !this.is_check_mode) {
          this.$set(
            this.second_elements_score_array,
            this.current_frame,
            this.score
          );
        }
        /*--------------Important---------------*/
        this.calculate_score_function();
        /*--------------Increase the current frame (important)---------------*/
        if (
          this.score === 10 &&
          this.ball_index === 1 &&
          this.current_frame < this.max_frame - 1
        ) {
          this.ball_index = 1;
          this.current_frame += 1;
        } else {
          if (this.ball_index === 1) {
            this.ball_index = 2;
          } else if (this.ball_index === 2) {
            this.ball_index = 1;
            this.current_frame += 1;
          }
        }
        /*--------------------------------------------endgame--------------------------------------------*/
        if (this.total_score_array[this.max_frame - 1] !== "-") {
          this.is_end = true;
        }
    },
    calculate_score_function() {
      if (this.current_frame === 0) {
        if (this.ball_index === 2) this.calculate_normal_case();
      } else if (this.current_frame > 0) {
        // Normal_frame
        if (this.ball_index === 1 && this.current_frame < this.max_frame) {
          this.calculate_combo_strike();
          this.calculate_spare_bonus();
        } else if (
          this.ball_index === 2 &&
          this.current_frame < this.max_frame
        ) {
          this.calculate_single_strike();
          this.calculate_normal_case();
        }
        // The_last_frame
        else if (
          this.ball_index === 1 &&
          this.current_frame === this.max_frame
        ) {
          if (this.first_elements_score_array[this.current_frame - 1] === 10) {
            this.$set(
              this.total_score_array,
              this.current_frame - 1,
              this.total_score_array[this.current_frame - 2] +
                10 +
                this.first_elements_score_array[this.current_frame] +
                this.second_elements_score_array[this.current_frame - 1]
            );
          } else {
            this.$set(
              this.total_score_array,
              this.current_frame - 1,
              this.total_score_array[this.current_frame - 2] +
                this.first_elements_score_array[this.current_frame - 1] +
                this.second_elements_score_array[this.current_frame - 1] +
                this.first_elements_score_array[this.current_frame]
            );
          }
          this.current_frame += 1;
        }
      }
    },
    /*---------------------------------------------------------------------------------------------------*/
    calculate_single_strike() {
      if (this.current_frame > 1) {
        if (this.first_elements_score_array[this.current_frame - 1] === 10) {
          this.$set(
            this.total_score_array,
            this.current_frame - 1,
            this.total_score_array[this.current_frame - 2] +
              10 +
              this.first_elements_score_array[this.current_frame] +
              this.second_elements_score_array[this.current_frame]
          );
        }
      } else if (this.current_frame === 1) {
        if (this.first_elements_score_array[this.current_frame - 1] === 10) {
          this.$set(
            this.total_score_array,
            this.current_frame - 1,
            10 +
              this.first_elements_score_array[this.current_frame] +
              this.second_elements_score_array[this.current_frame]
          );
        }
      }
    },
    /*---------------------------------------------------------------------------------------------------*/
    calculate_combo_strike() {
      if (
        this.first_elements_score_array[this.current_frame - 1] === 10 &&
        this.first_elements_score_array[this.current_frame - 2] === 10
      ) {
        if (this.current_frame > 2) {
          this.$set(
            this.total_score_array,
            this.current_frame - 2,
            this.total_score_array[this.current_frame - 3] +
              20 +
              this.first_elements_score_array[this.current_frame]
          );
        } else if (this.current_frame === 2) {
          this.$set(
            this.total_score_array,
            this.current_frame - 2,
            20 + this.first_elements_score_array[this.current_frame]
          );
        }
      }
    },
    /*---------------------------------------------------------------------------------------------------*/
    calculate_normal_case() {
      if (
        this.first_elements_score_array[this.current_frame] !== 10 &&
        this.first_elements_score_array[this.current_frame] +
          this.second_elements_score_array[this.current_frame] !==
          10
      ) {
        if (this.current_frame > 0) {
          this.$set(
            this.total_score_array,
            this.current_frame,
            this.total_score_array[this.current_frame - 1] +
              this.first_elements_score_array[this.current_frame] +
              this.second_elements_score_array[this.current_frame]
          );
        } else if (this.current_frame === 0) {
          this.$set(
            this.total_score_array,
            this.current_frame,
            this.first_elements_score_array[this.current_frame] +
              this.second_elements_score_array[this.current_frame]
          );
        }
      }
    },
    /*---------------------------------------------------------------------------------------------------*/
    calculate_spare_bonus() {
      if (
        this.first_elements_score_array[this.current_frame - 1] +
          this.second_elements_score_array[this.current_frame - 1] ===
          10 &&
        this.second_elements_score_array[this.current_frame - 1] !== 0
      ) {
        if (this.current_frame === 1) {
          this.$set(
            this.total_score_array,
            this.current_frame - 1,
            this.first_elements_score_array[this.current_frame - 1] +
              this.second_elements_score_array[this.current_frame - 1] +
              this.first_elements_score_array[this.current_frame]
          );
        } else if (this.current_frame > 1) {
          this.$set(
            this.total_score_array,
            this.current_frame - 1,
            this.total_score_array[this.current_frame - 2] +
              this.first_elements_score_array[this.current_frame - 1] +
              this.second_elements_score_array[this.current_frame - 1] +
              this.first_elements_score_array[this.current_frame]
          );
        }
      }
    },
    /*---------------------------------------------------------------------------------------------------*/
    caculate_pins_fall_function(ball_offset) {
      var pins_ratio_array = Array();
      var game_rect = document
        .getElementById("game-div")
        .getBoundingClientRect();
      for (var i = 1; i < 11; i++) {
        var pin_rect = document
          .getElementById("pin-" + i)
          .getBoundingClientRect();
        if (!this.pins_fall_array[i - 1]) {
          pins_ratio_array.push(
            (ball_offset-2) / (pin_rect.left - game_rect.left)
          );
        } else {
          pins_ratio_array.push(0);
        }
      }
      /*The ratio position threshold*/
      var goal = 0.9;
      var var_score = 0;
      //
      var closest = pins_ratio_array.reduce(function (prev, curr) {
        return Math.abs(curr - goal) < Math.abs(prev - goal) ? curr : prev;
      });
      var index_of_nearest_ball = pins_ratio_array.indexOf(closest);
      if (closest > 0.85 && closest < 0.96) {
        if (
          index_of_nearest_ball + 1 === 1 ||
          index_of_nearest_ball + 1 === 5
        ) {
          var random =
            this.random_1_5[Math.floor(Math.random() * this.random_1_5.length)];
          for (let pin of this.pin_1_5) {
            if (!this.pins_fall_array[pin]) {
              this.current_pins -= 1;
              var_score += 1;
              this.pins_fall_array[pin] = true;
              window.requestAnimationFrame(function (time) {
                window.requestAnimationFrame(function (time) {
                  document.getElementById("pin-" + (pin + 1)).className =
                    "pin-div-" + (pin + 1) + " pin-animation";
                });
              });
            }
            random -= 1;
            if (random === 0) break;
          }
        } else if (
          index_of_nearest_ball + 1 === 3 ||
          index_of_nearest_ball + 1 === 8
        ) {
          var random =
            this.random_2_9_3_8[Math.floor(Math.random() * this.random_2_9_3_8.length)];

          for (let item of this.pin_3_8) {
            if (!this.pins_fall_array[item - 1]) {
              var_score += 1;
              this.current_pins -= 1;
              this.pins_fall_array[item - 1] = true;
              window.requestAnimationFrame(function (time) {
                window.requestAnimationFrame(function (time) {
                  document.getElementById("pin-" + item).className =
                    "pin-div-" + item + " pin-animation";
                });
              });
            }
            random -= 1;
            if (random === 0) break;
          }
        } else if (
          index_of_nearest_ball + 1 === 2 ||
          index_of_nearest_ball + 1 === 9
        ) {
          var random =
            this.random_2_9_3_8[Math.floor(Math.random() * this.random_2_9_3_8.length)];
          for (let item of this.pin_2_9) {
            if (!this.pins_fall_array[item - 1]) {
              var_score += 1;
              this.current_pins -= 1;
              this.pins_fall_array[item - 1] = true;
              window.requestAnimationFrame(function (time) {
                window.requestAnimationFrame(function (time) {
                  document.getElementById("pin-" + item).className =
                    "pin-div-" + item + " pin-animation";
                });
              });
            }
            random -= 1;
            if (random === 0) break;
          }
        } else if (index_of_nearest_ball + 1 === 4) {
          var random =
            this.random_4_6[Math.floor(Math.random() * this.random_4_6.length)];
          for (let item of this.pin_4) {
            if (!this.pins_fall_array[item - 1]) {
              var_score += 1;
              this.current_pins -= 1;
              this.pins_fall_array[item - 1] = true;
              window.requestAnimationFrame(function (time) {
                window.requestAnimationFrame(function (time) {
                  document.getElementById("pin-" + item).className =
                    "pin-div-" + item + " pin-animation";
                });
              });
            }
            random -= 1;
            if (random === 0) break;
          }
        } else if (index_of_nearest_ball + 1 === 6) {
          var random =
            this.random_4_6[Math.floor(Math.random() * this.random_4_6.length)];
          for (let item of this.pin_6) {
            if (!this.pins_fall_array[item - 1]) {
              var_score += 1;
              this.current_pins -= 1;
              this.pins_fall_array[item - 1] = true;
              window.requestAnimationFrame(function (time) {
                window.requestAnimationFrame(function (time) {
                  document.getElementById("pin-" + item).className =
                    "pin-div-" + item + " pin-animation";
                });
              });
            }
            random -= 1;
            if (random === 0) break;
          }
        } else if (
          index_of_nearest_ball + 1 === 7 ||
          (index_of_nearest_ball + 1 === 10 && closest < 0.95)
        ) {
          if (!this.pins_fall_array[index_of_nearest_ball]) {
            var_score += 1;
            this.current_pins -= 1;
            this.pins_fall_array[index_of_nearest_ball] = true;
            window.requestAnimationFrame(function (time) {
              window.requestAnimationFrame(function (time) {
                document.getElementById(
                  "pin-" + (index_of_nearest_ball + 1)
                ).className =
                  "pin-div-" + (index_of_nearest_ball + 1) + " pin-animation";
              });
            });
          }
        }
      }
      this.score = var_score;
      if (this.score < 10) {
        this.count_ball += 1;
      } else {
        this.count_ball += 2;
      }
      const reset_pin_timeout = setTimeout(this.reset_pin, 4000);
    },
    /*---------------------------------------------------------------------------------------------------*/
    reset_pin() {
      document.getElementById("red-ball").style.display = "block";
      document.getElementById("black-ball").style.display = "block";
      this.was_thrown = false;
      if (this.count_ball >= 2) {
        for (var i = 1; i < 11; i++) {
          document.getElementById("pin-" + i).className = "pin-div-" + i;
        }
        this.count_ball = 0;
        this.current_pins = 10;
        this.pins_fall_array = Array(10).fill(false);
      }
    },
  },
};
/*---------------------------------------------------------------------------------------------------*/
</script>

<style scoped>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
.game-div {
  position: absolute;
  top: 5px;
  left: calc((100vw - 600px) / 2);
  width: 600px;
  height: 900px;
  max-height: calc(100vh - 5px);
  background-color: rgb(217, 215, 223);
  border-radius: 2px;
  overflow: hidden;
  font-size: small;
  font-family: "Rubik", sans-serif;
}
.nav-bar-div {
  position: absolute;
  top: 5px;
  left: 0%;
  width: 98%;
  height: 25px;
  left: 1%;
  border-radius: 5px 5px 0 0;
  background-color: white;
  display: flex;
  flex-wrap: wrap;
  padding-top: 1px;
  padding-right: 1px;
  z-index: 1000;
  display: inline-flex;
  gap: 1%;
}
.score-board {
  position: absolute;
  top: 25px;
  left: 0%;
  width: 98%;
  height: fit-content;
  margin: 1%;
  background-color: white;
  display: flex;
  flex-wrap: wrap;
  padding-top: 1px;
  padding-right: 1px;
  z-index: 1000;
}
.disable-drag {
  user-select: none;
  -moz-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
.link-btn {
  padding: 2px;
  margin-top: 2px;
  margin-left: 3px;
  outline: none !important;
  border: none;
  height: fit-content;
  border-radius: 4px;
  background-color: rgb(166, 216, 231);
  border: 1px solid rgb(129, 168, 180);
  text-decoration: none;
  color: #163024;
  font-size: small;
  font-family: "Rubik", sans-serif;
}
.link-btn:hover {
  background-color: #5f8675;
  color: white;
  cursor: pointer;
}
.item-1 {
  background-color: rgb(219, 199, 195);
  width: 8%;
}
.item-2 {
  overflow: hidden;
  width: 20%;
}
.item-3 {
  width: 8%;
}
.table-label-div {
  width: 100%;
  height: 33.33%;
  white-space: nowrap;
  overflow: hidden;
  font-size: small;
}
.border-inside {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  border-left: 1px solid white;
  border-bottom: 1px solid white;
}
.double-col {
  display: flex;
  flex-direction: row;
}
.element-score-div {
  width: 50%;
}
.ball-trench {
  position: absolute;
  width: 69%;
  height: 169px;
  bottom: 100px;
  margin-left: 15.2%;
  background-color: rgb(61, 50, 50);
  -webkit-clip-path: polygon(39.5% 0, 0 100%, 100% 100%, 100% 100%, 68% 0);
  clip-path: polygon(39.5% 0, 0 100%, 100% 100%, 100% 100%, 68% 0);
  z-index: 100;
}
.the-bowling-state {
  position: absolute;
  width: 73%;
  height: 269px;
  bottom: 0;
  border-radius: 0 0 5px 5px;
  margin-left: 12%;
  margin-right: 15%;
  background-color: white;
  -webkit-clip-path: polygon(46% 0, 0 100%, 100% 100%, 100% 100%, 64% 0);
  clip-path: polygon(46% 0, 0 100%, 100% 100%, 100% 100%, 64% 0);
  z-index: 100;
}
.pin-place-div {
  background-color: rgb(192, 187, 181);
  height: 18px;
}
.front-floor-div {
  position: absolute;
  bottom: 0px;
  left: 0%;
  width: 100%;
  height: 100px;
  background-color: rgb(88, 118, 119);
}
.middle-floor-div {
  position: absolute;
  bottom: 100px;
  left: 0%;
  width: 100%;
  height: 169px;
  background-color: lightsteelblue;
  -webkit-clip-path: polygon(34% 0, 0 100%, 100% 100%, 100% 100%, 70% 0);
  clip-path: polygon(34% 0, 0 100%, 100% 100%, 100% 100%, 70% 0);
}
.bowling-basket-div {
  position: absolute;
  right: 0px;
  bottom: 29px;
  height: 100px;
  width: 15%;
  background-color: rgb(75, 75, 75);
  border-radius: 5px 0px 0px 0;
  border-top: 20px solid rgb(119, 118, 118);
  z-index: 100;
}
.bowling-basket {
  position: absolute;
  right: 0px;
  bottom: -0%;
  height: 130px;
  width: 15%;
  background-color: rgb(141, 135, 125);
  z-index: 100;
  transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg) translateX(0px)
    translateZ(-105px);
  clip-path: polygon(0 0, 95% 100%, 100% 100%, 100% 100%, 100% 0);
}
.pins-hole-div {
  position: absolute;
  bottom: 269px;
  width: 19.7%;
  height: 40px;
  border-radius: 25px 25px 0 0;
  background-color: rgb(41, 32, 32);
  left: 42.3%;
  z-index: 100;
}
.enable-select {
  user-select: none;
  -moz-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
.pin-div-1 {
  position: absolute;
  bottom: 250px;
  width: 12px;
  height: fit-content;
  left: calc((100% - 12px) / 2 + 2%);
  z-index: 103;
}
.pin-div-2 {
  position: absolute;
  bottom: 255px;
  width: 12px;
  height: fit-content;
  z-index: 102;
  left: calc((100% - 12px) / 2 + 4%);
}
.pin-div-3 {
  position: absolute;
  bottom: 255px;
  width: 12px;
  height: fit-content;
  z-index: 102;
  left: calc((100% - 12px) / 2 + 0%);
}
.pin-div-4 {
  position: absolute;
  bottom: 260px;
  width: 12px;
  height: fit-content;
  z-index: 101;
  left: calc((100% - 12px) / 2 - 2%);
}
.pin-div-5 {
  position: absolute;
  bottom: 260px;
  width: 12px;
  height: fit-content;
  z-index: 101;
  left: calc((100% - 12px) / 2 + 2%);
}
.pin-div-6 {
  position: absolute;
  bottom: 260px;
  width: 12px;
  height: fit-content;
  z-index: 101;
  left: calc((100% - 12px) / 2 + 6%);
}
.pin-div-7 {
  position: absolute;
  bottom: 265px;
  width: 12px;
  height: fit-content;
  z-index: 100;
  left: calc((100% - 12px) / 2 - 3.8%);
}
.pin-div-8 {
  position: absolute;
  bottom: 265px;
  width: 12px;
  height: fit-content;
  z-index: 100;
  left: calc((100% - 12px) / 2 + 0.2%);
}

.pin-div-9 {
  position: absolute;
  width: 12px;
  height: fit-content;
  z-index: 100;
  bottom: 265px;
  left: calc((100% - 12px) / 2 + 4%);
}
.pin-div-10 {
  position: absolute;
  bottom: 265px;
  width: 12px;
  height: fit-content;
  z-index: 100;
  left: calc((100% - 12px) / 2 + 8%);
}

.back-wall-div {
  position: absolute;
  bottom: 269px;
  width: 36%;
  height: calc(100% - 269px);
  background-color: rgb(248, 241, 241);
  left: 34%;
}
.the-red-ball {
  position: absolute;
  right: -0.5em;
  bottom: 90px;
  height: 4.5em;
  width: 4.5em;
  border-radius: 50%;
  z-index: 999;
  cursor: pointer;
}
.the-red-ball:hover {
  border: 1px solid rgb(187, 253, 250);
  height: 5em;
  width: 5em;
}
.the-black-ball {
  position: absolute;
  right: -2.5em;
  bottom: 50px;
  height: 5em;
  width: 5em;
  border-radius: 50%;
  z-index: 999;
  cursor: pointer;
}
.the-black-ball:hover {
  border: 1px solid rgb(187, 253, 250);
  height: 5.5em;
  width: 5.5em;
}

.shadow-of-the-basket-div {
  position: absolute;
  right: 0px;
  bottom: 0;
  height: 30px;
  width: 15%;
  background-color: rgb(75, 75, 75);
  -webkit-clip-path: polygon(0 0, 20% 100%, 100% 100%, 100% 100%, 100% 0);
  clip-path: polygon(0 0, 20% 100%, 100% 100%, 100% 100%, 100% 0);
}
.spining-ball {
  position: absolute;
  bottom: 80px;
  height: 5em;
  width: 5em;
  border-radius: 50%;
  z-index: 100;
  cursor: pointer;
  opacity: 0;
}
.animation-red-ball {
  animation: bowling-roll ease 3s;
}
.animation-black-ball {
  animation: bowling-roll ease 3s;
}
.enter-the-black-ball {
  position: absolute;
  right: 40%;
  bottom: 50px;
  height: 4em;
  width: 4em;
  border-radius: 50%;
  z-index: 999;
  cursor: pointer;
  display: none;
}
.enter-the-red-ball {
  position: absolute;
  right: 40%;
  bottom: 50px;
  height: 4em;
  width: 4em;
  border-radius: 50%;
  z-index: 999;
  cursor: pointer;
  display: none;
}
.throw-the-ball-animation {
  animation: throw-the-ball 0.5s ease-in-out alternate infinite;
}
.enter-btn {
  position: absolute;
  right: 80%;
  bottom: 70px;
  height: 4em;
  width: 4em;
  background-color: rgb(0, 90, 83);
  border-radius: 50%;
  z-index: 999;
  cursor: pointer;
}
.ball-track {
  position: absolute;
  right: 30%;
  bottom: 50px;
  height: 3.8em;
  width: calc(30% + 4em);
  background-color: rgb(166, 206, 209);
  border-radius: 25px;
  border: 2px solid rgb(148, 116, 48);
  z-index: 999;
  opacity: 0.7;
  cursor: pointer;
  display: none;
}
.pin-animation {
  animation: pin-fall 1.5s;
  animation-delay: 1.8s;
  animation-fill-mode: forwards;
}
.game-over-div {
  position: absolute;
  top: 2.8em;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0.7;
  z-index: 2000;
  background-color: black;
  color: cadetblue;
  font-size: xx-large;
  padding-top: 10%;
}
.check-mode-div {
  position: absolute;
  top: 2.8em;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0.3;
  z-index: 1000;
  background-color: black;
  color: cadetblue;
  font-size: xx-large;
  padding-top: 40%;
}
/* button mẫu */
.button-30 {
  position: absolute;
  right: 75%;
  bottom: 70px;
  height: 4em;
  width: 4em;
  background-color: rgb(43, 155, 163);
  z-index: 999;
  border-radius: 25px;
  align-items: center;
  appearance: none;
  border-width: 0;
  box-shadow: rgba(45, 35, 66, 0.4) 0 2px 4px,
    rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -3px 0 inset;
  box-sizing: border-box;
  color: #ffffff;
  cursor: pointer;
  display: inline-flex;
  justify-content: center;
  font-family: "Rubik", sans-serif;
  line-height: 1;
  list-style: none;
  overflow: hidden;
  padding-left: 16px;
  padding-right: 16px;
  text-align: left;
  text-decoration: none;
  transition: box-shadow 0.15s, transform 0.15s;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  white-space: nowrap;
  will-change: box-shadow, transform;
  font-size: 18px;
}

.button-30:focus {
  box-shadow: #d6d6e7 0 0 0 1.5px inset, rgba(45, 35, 66, 0.4) 0 2px 4px,
    rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -3px 0 inset;
}

.button-30:hover {
  box-shadow: rgba(45, 35, 66, 0.4) 0 4px 8px,
    rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -3px 0 inset;
  transform: translateY(-2px);
}

.button-30:active {
  box-shadow: #d6d6e7 0 3px 7px inset;
  transform: translateY(2px);
}
@keyframes pin-fall {
  0% {
    transform: rotateX(0deg) translateZ(0);
    opacity: 1;
  }
  99.99% {
    opacity: 0.2;
  }
  100% {
    transform: rotateX(-60deg) translateZ(10px) rotateY(20deg);
    opacity: 0;
  }
}
@keyframes throw-the-ball {
  0% {
    right: 60%;
  }
  100% {
    right: 30%;
  }
}
@keyframes bowling-roll {
  0% {
    opacity: 1;
    bottom: 0px;
    transform: scale(1) rotateZ(0);
  }
  20% {
    bottom: 50px;
  }
  20% {
    bottom: 60px;
    /* left: 50%; */
    transform: scale(0.8) rotateZ(120deg);
  }
  99.99% {
    opacity: 1;
    display: none !important;
  }
  100% {
    bottom: 250px;
    transform: scale(0.2) rotateZ(720deg);
    display: none !important;
    opacity: 0;
  }
}
@media only screen and (max-width: 1263px) {
  .game-div {
    top: 0%;
    left: calc((100% - 600px) / 2);
    width: 600px;
    height: 900px;
    max-height: 100%;
  }
}
@media only screen and (max-width: 600px) {
  .game-div {
    top: 0;
    left: 0vw;
    width: 100vw;
    height: 100%;
    font-size: small;
  }
  .spining-ball {
    bottom: 80px;
    height: 3.5em;
    width: 3.5em;
  }
  .the-red-ball {
    right: 0.25em;
    bottom: 90px;
    height: 3.75em;
    width: 3.75em;
  }
  .the-black-ball {
    right: -1.5em;
    height: 4em;
    width: 4em;
  }
  .table-label-div {
    font-size: x-small;
  }
}
</style>
