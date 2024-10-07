<template>
  <div class="card">
    <div class="d-flex justify-content-between">
      <div class="winner-tags gray-border">
        <span
          v-for="(winner, index) in winners"
          :key="index"
          class="badge blue"
        >
          {{ winner.name }}
          <button @click="removeWinner(index)" class="btn btn-sm btn-danger">
            &times;
          </button>
        </span>
        <span class="badge">Winners</span>
      </div>
      <button
        class="btn btn-primary"
        :disabled="winners.length >= 3 || participants.length === 0"
        @click="selectWinner"
      >
        New winner
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { Participant } from "@/models/Participant";

export default defineComponent({
  name: "WinnerManager",
  props: {
    participants: {
      type: Array as () => Participant[],
      required: true,
    },
    winners: {
      type: Array as () => Participant[],
      required: true,
    },
  },
  emits: ["winnerSelected", "winnerRemoved"],
  methods: {
    selectWinner() {
      const randomIndex = Math.floor(Math.random() * this.participants.length);
      const winner = this.participants[randomIndex];
      this.$emit("winnerSelected", winner);
    },
    removeWinner(index: number) {
      this.$emit("winnerRemoved", index);
    },
  },
});
</script>

<style scoped>
.card {
  border: 1px solid #dee2e6;
  border-radius: 8px;
  background-color: #f8f9fa;
  padding: 3%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.d-flex {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.winner-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.winner-tags .badge {
  display: flex;
  color: black;
  height: 35px;
  align-items: center;
  gap: 5px;
  padding: 10px;
  font-size: 1rem;
}

.blue {
  background-color: rgba(13, 110, 253, 0.5);
}

.btn-danger {
  background-color: white;
  color: black;
  font-size: 15px;
  border-radius: 50%;
  border: none;
}

.winner-tags button {
  margin-left: 2px;
  margin-top: 1px;
}

.gray-border {
  border: solid gray 1px;
  border-radius: 10px;
  padding: 10px;
}

.btn-primary {
  margin-left: 20px;
}
</style>
