<template>
  <div class="lottery-app">
    <WinnerManager
      :participants="participants"
      :winners="winners"
      @winnerSelected="addWinner"
      @winnerRemoved="removeWinner"
    />
    <ParticipantForm @participantRegistered="registerParticipant" />
    <ParticipantTable :participants="participants" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { Participant } from "@/models/Participant";
import WinnerManager from "@/components/WinnerManager.vue";
import ParticipantForm from "@/components/ParticipantForm.vue";
import ParticipantTable from "@/components/ParticipantTable.vue";

export default defineComponent({
  name: "LotteryApp",
  components: {
    WinnerManager,
    ParticipantForm,
    ParticipantTable,
  },
  setup() {
    const participants = ref<Participant[]>([
      // Sample participants
    ]);
    const winners = ref<Participant[]>([]);

    const registerParticipant = (participant: Participant) => {
      participants.value.push(participant);
    };

    const addWinner = (winner: Participant) => {
      winners.value.push(winner);
      participants.value = participants.value.filter((p) => p !== winner);
    };

    const removeWinner = (index: number) => {
      const winner = winners.value.splice(index, 1)[0];
      participants.value.push(winner);
    };

    return {
      participants,
      winners,
      registerParticipant,
      addWinner,
      removeWinner,
    };
  },
});
</script>

<style scoped>
/** {
  border: solid green 1px;
}*/

.lottery-app {
  display: grid;
  grid-template-columns: 1fr;
  gap: 20px;
  max-width: 800px;
  margin: 0 auto;
}

.card {
  border: 1px solid #dee2e6;
  border-radius: 8px;
  background-color: #f8f9fa;
  padding: 3%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.d-flex {
  display: flex;
  flex-direction: column;
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

.winner-tags button {
  margin-left: 2px;
  margin-top: 1px;
}

.gray-border {
  border: solid gray 1px;
  border-radius: 10px;
  padding: 10px;
}

form div {
  margin-bottom: 10px;
}

.text-danger {
  color: red;
  font-size: 0.875rem;
}

table {
  width: 100%;
}

thead {
  background-color: #e9ecef;
}

th,
td {
  padding: 8px;
  text-align: center;
}

button {
  margin-top: 10px;
}
</style>
