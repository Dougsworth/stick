<template>
  <div class="container">
    <h1 class="fade-in">Who is holding the stick this month?</h1>
    <div class="bold fade-in">Stick Holder: {{ stickHolder }}</div>
    <div class="bold fade-in">Current Month: {{ currentMonthName }}</div>

    <!-- Countdown Timer -->
    <div class="bold fade-in">
      Time left this month: {{ countdown.days }}d {{ countdown.hours }}h
      {{ countdown.minutes }}m {{ countdown.seconds }}s
    </div>

    <!-- Stick Card -->
    <div class="card slide-in">
      <h2 class="card-title">The Stick</h2>
      <div class="stick-animation">
        <img
          src="/colored-wooden-stick-105676.png"
          alt="Stick"
          class="stick"
          @click="handleStickClick"
          :class="{ clicked: stickClicked }"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      months: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ],
      currentMonth: new Date().getMonth(),
      currentDate: new Date(),
      countdown: {
        days: 0,
        hours: 0,
        minutes: 0,
        seconds: 0,
      },
      stickClicked: false, // Track stick click state
    };
  },
  computed: {
    stickHolder() {
      return this.currentMonth % 2 === 0 ? "Marika" : "Douglas";
    },
    currentMonthName() {
      return this.months[this.currentMonth];
    },
  },
  mounted() {
    this.calculateCountdown();
    // Update countdown every second
    setInterval(() => {
      this.calculateCountdown();
    }, 1000);
  },
  methods: {
    calculateCountdown() {
      const now = new Date();
      const nextMonth = new Date(now.getFullYear(), now.getMonth() + 1, 1); // First day of next month
      const timeDiff = nextMonth - now;

      const days = Math.floor(timeDiff / (1000 * 60 * 60 * 24));
      const hours = Math.floor(
        (timeDiff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
      );
      const minutes = Math.floor((timeDiff % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((timeDiff % (1000 * 60)) / 1000);

      this.countdown = { days, hours, minutes, seconds };
    },
    handleStickClick() {
      // Trigger the stick click animation
      this.stickClicked = true;

      // Reset after the animation duration (e.g., 1 second)
      setTimeout(() => {
        this.stickClicked = false;
      }, 1000); // Adjust timing to match the CSS animation duration
    },
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background-color: #f0f8ff;
  animation: fadeInBody 1.5s ease-in-out;
}

.container {
  text-align: center;
}

/* Fade-in effect for content */
.fade-in {
  opacity: 0;
  animation: fadeIn 1.5s forwards;
}

.bold {
  font-size: 2rem;
  font-weight: bold;
  margin: 20px 0;
  color: #333;
}

h1 {
  font-size: 1.5rem;
  color: #444;
}

/* Card styling for stick */
.card {
  background: #fff;
  border-radius: 15px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
  max-width: 500px;
  margin: 20px auto;
  text-align: center;
  opacity: 0;
  transform: translateY(20px);
  animation: slideIn 1s ease-in forwards;
}

.card-title {
  font-size: 1.75rem;
  margin-bottom: 10px;
  color: #444;
}

.stick-animation {
  position: relative;
  width: 100%;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 10px;
}

.stick {
  width: 150px;
  height: auto;
  animation: moveStick 2s infinite alternate;
  cursor: pointer;
  transition: transform 0.5s ease, rotate(0.5s) ease;
}

/* Clicked stick animation */
.stick.clicked {
  transform: scale(1.3) rotate(360deg);
}

/* Keyframes for stick animation */
@keyframes moveStick {
  0% {
    transform: translateX(-50px);
  }
  100% {
    transform: translateX(50px);
  }
}

/* Fade-in animation for content */
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

/* Slide-in animation for card */
@keyframes slideIn {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Fade-in for body background */
@keyframes fadeInBody {
  0% {
    background-color: #f0f8ff;
  }
  100% {
    background-color: #e0f7fa;
  }
}

/* Responsive styling */
@media (max-width: 768px) {
  .card {
    max-width: 90%;
  }

  .bold {
    font-size: 1.5rem;
  }

  .stick {
    width: 100px;
  }

  .stick-animation {
    height: 80px;
  }
}

@media (max-width: 480px) {
  .bold {
    font-size: 1.2rem;
  }

  .stick {
    width: 80px;
  }

  .stick-animation {
    height: 60px;
  }
}
</style>
