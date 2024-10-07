<template>
  <div class="card">
    <h3>REGISTER FORM</h3>
    <p>Please fill in all the fields.</p>
    <form @submit.prevent="submitForm" novalidate>
      <div class="form-group">
        <label>Name</label>
        <input
          v-model="participant.name"
          type="text"
          class="form-control"
          placeholder="Enter user name"
          :class="{ 'is-invalid': errors.name }"
          required
        />
        <div class="text-danger" v-if="errors.name">{{ errors.name }}</div>
      </div>
      <div class="form-group">
        <label>Date of Birth</label>
        <input
          v-model="participant.dateOfBirth"
          type="date"
          class="form-control"
          :max="today"
          :class="{ 'is-invalid': errors.dateOfBirth }"
          required
        />
        <div class="text-danger" v-if="errors.dateOfBirth">
          {{ errors.dateOfBirth }}
        </div>
      </div>
      <div class="form-group">
        <label>Email</label>
        <input
          v-model="participant.email"
          type="email"
          class="form-control"
          placeholder="Enter email"
          :class="{ 'is-invalid': errors.email }"
          required
        />
        <div class="text-danger" v-if="errors.email">{{ errors.email }}</div>
      </div>
      <div class="form-group">
        <label>Phone number</label>
        <input
          v-model="participant.phoneNumber"
          type="tel"
          class="form-control"
          placeholder="Enter phone number"
          :class="{ 'is-invalid': errors.phoneNumber }"
          required
        />
        <div class="text-danger" v-if="errors.phoneNumber">
          {{ errors.phoneNumber }}
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Save</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { Validator } from "@/misc/Validator";
import { Participant } from "@/models/Participant";

export default defineComponent({
  name: "ParticipantForm",
  emits: ["participantRegistered"],
  setup(props, { emit }) {
    const today = new Date().toISOString().split("T")[0];
    const participant = ref<Participant>({
      name: "",
      dateOfBirth: "",
      email: "",
      phoneNumber: "",
    });

    const errors = ref({
      name: "",
      dateOfBirth: "",
      email: "",
      phoneNumber: "",
    });

    const submitForm = () => {
      errors.value.name = Validator.validateName(participant.value.name);
      errors.value.dateOfBirth = Validator.validateDateOfBirth(
        participant.value.dateOfBirth,
        today
      );
      errors.value.email = Validator.validateEmail(participant.value.email);
      errors.value.phoneNumber = Validator.validatePhoneNumber(
        participant.value.phoneNumber
      );

      if (
        !errors.value.name &&
        !errors.value.dateOfBirth &&
        !errors.value.email &&
        !errors.value.phoneNumber
      ) {
        emit("participantRegistered", { ...participant.value });
        participant.value = {
          name: "",
          dateOfBirth: "",
          email: "",
          phoneNumber: "",
        };
      }
    };

    return {
      participant,
      errors,
      today,
      submitForm,
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
