<template>
  <h1>Jobs carousel</h1>
  <div class="wrapper">
    <div class="pick-job-carousel">
      <div class="inner" ref="inner" :style="innerStyles">
        <div v-for="(job, index) in jobs" :key="job.id" class="card">
          <JobDetails
            :data="job"
            :selected="selectedJob === jobs[index] ? true : false"
            @select="selectedJob = jobs[index]"
          />
        </div>
      </div>
    </div>
    <div class="scrolling-btns">
      <button @click="left()">Left</button>
      <button @click="right()">Right</button>
    </div>
    <button @click="handleNextClick()" class="submit">Submit</button>
  </div>
</template>

<script>
import JobDetails from "./JobDetails.vue";

export default {
  name: "JobsView",
  components: {
    JobDetails,
  },
  data() {
    return {
      jobs: [
        {
          title: "Ninja UX Designer",
          id: 1,
          details:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum quis arcu vitae mi aliquet finibus. Proin massa quam, luctus a interdum a, mollis sit amet purus. Vestibulum sagittis, felis vitae egestas vestibulum, sem magna convallis ex, lobortis facilisis nisl purus non eros. Ut ut bibendum nunc. Phasellus laoreet rutrum vulputate. Etiam a tristique lacus. Nunc pellentesque orci elit, in tempus arcu sodales non. Ut et laoreet nulla.",
        },
        {
          title: "Ninja Web Developer",
          id: 2,
          details:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum quis arcu vitae mi aliquet finibus. Proin massa quam, luctus a interdum a, mollis sit amet purus. Vestibulum sagittis, felis vitae egestas vestibulum, sem magna convallis ex, lobortis facilisis nisl purus non eros. Ut ut bibendum nunc. Phasellus laoreet rutrum vulputate. Etiam a tristique lacus. Nunc pellentesque orci elit, in tempus arcu sodales non. Ut et laoreet nulla.",
        },
        {
          title: "Ninja Web Designer",
          id: 3,
          details:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum quis arcu vitae mi aliquet finibus. Proin massa quam, luctus a interdum a, mollis sit amet purus. Vestibulum sagittis, felis vitae egestas vestibulum, sem magna convallis ex, lobortis facilisis nisl purus non eros. Ut ut bibendum nunc. Phasellus laoreet rutrum vulputate. Etiam a tristique lacus. Nunc pellentesque orci elit, in tempus arcu sodales non. Ut et laoreet nulla.",
        },
      ],
      selectedJob: null,
      selectedJobIndex: 1,
      innerStyles: {},
      step: "",
      isTransitioning: false,
    };
  },
  mounted() {
    this.selectedJob = this.jobs[1];
    this.setStep();
    this.resetTranslate();
  },
  methods: {
    select() {
      this.$emit("select");
    },
    handleNextClick() {
      console.log("selected job: ", this.selectedJob);
      console.log("hm", this.jobs[0] === this.selectedJob);
    },
    setStep() {
      const innerWidth = this.$refs.inner.scrollWidth;
      const totalCards = this.jobs.length;
      console.log(innerWidth);
      this.step = `${innerWidth / totalCards}px`;
      console.log(this.step);
    },
    right() {
      if (this.isTransitioning) return;

      this.isTransitioning = true;
      console.log("next clicked");

      this.scroll("right");
      this.afterTransition(() => {
        const job = this.jobs.shift();
        this.jobs.push(job);
        this.resetTranslate();
        this.changeSelected("next");
        this.isTransitioning = false;
      });
    },
    left() {
      if (this.transitioning) return;

      this.transitioning = true;
      console.log("prev clicked");

      this.scroll("left");
      this.afterTransition(() => {
        const job = this.jobs.pop();
        this.jobs.unshift(job);
        this.resetTranslate();
        this.changeSelected("prev");
        this.transitioning = false;
      });
    },
    scroll(direction) {
      if (direction === "right") {
        this.innerStyles = {
          transform: `translateX(-${this.step})
                translateX(-${this.step})`, // ❶
        };
      } else {
        this.innerStyles = {
          transform: `translateX(${this.step})
                translateX(-${this.step})`, // ❷
        };
      }
    },
    afterTransition(callback) {
      const listener = () => {
        callback();
        this.$refs.inner.removeEventListener("transitionend", listener);
      };
      this.$refs.inner.addEventListener("transitionend", listener);
    },
    resetTranslate() {
      this.innerStyles = {
        transition: "none",
        transform: `translateX(-${this.step})`,
      };
    },
    changeSelected(direction) {
      console.log("before click selected: ", this.selectedJobIndex);
      console.log("jobs length: ", this.jobs.length);
      if (direction === "next") {
        if (this.selectedJobIndex < this.jobs.length - 1) {
          this.selectedJobIndex += 1;
        } else {
          this.selectedJobIndex = 0;
        }
      } else if (direction === "right") {
      }
      console.log("after click selected: ", this.selectedJobIndex);
      this.selectedJobIndex += 1;
      this.selectedJob = this.jobs[this.selectedJobIndex];
    },
  },
};
</script>

<style scoped>
.wrapper {
  position: relative;
}
.pick-job-carousel {
  width: 350px;
  margin: auto;
  overflow: hidden;
}
.inner {
  white-space: nowrap;
  transition: transform 0.5s ease;
}
.card {
  display: inline-flex;
}
.scrolling-btns {
  position: absolute;
  top: 40%;
  width: 70%;
  display: flex;
  justify-content: space-between;
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
}
.scrolling-btns button {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  background-color: white;
  border: none;
  filter: drop-shadow(2px 4px 6px black);
  cursor: pointer;
}
button.submit {
  margin: 50px auto;
  height: 40px;
  width: 120px;
}
@media (max-width: 700px) {
  .pick-job-carousel {
    width: 360px;
    margin: auto;
  }
  .scrolling-btns {
    width: 100%;
    margin-left: 0;
    margin-right: 0;
  }
}
</style>
