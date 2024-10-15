<script setup>
import { ref } from "vue";

const isCredentialFormCompleted = ref(false);
const firstname = ref("");
const lastname = ref("");
const height = ref("");
const weight = ref("");
const BodyMassIndex = ref(null);
const gender = ref("");

const submitCredentialsForm = () => {
  if (firstname.value !== "" && lastname.value !== "") {
    isCredentialFormCompleted.value = true;
  } else {
    alert("Please fill in both first name and last name.");
  }
};

const submitBodyReadings = () => {
  const heightInMeters = parseFloat(height.value);
  const weightInKg = parseFloat(weight.value);

  if (heightInMeters > 0 && weightInKg > 0) {
    // Calculate BMI
    BodyMassIndex.value = weightInKg / (heightInMeters * heightInMeters);

    alert("Body readings submitted successfully.");

    // Clear only height and weight, keep BMI to display it
    height.value = "";
    weight.value = "";

    isCredentialFormCompleted.value = false;

  } else {
    alert("Please fill in both height and weight.");
  }
};

// Suggestions based on BMI
const getBMISuggestions = (bmi, firstname, lastname, gender) => {
  let suggestion;

  if (bmi < 18.5) {
    suggestion = `You are underweight. Consider consulting a healthcare provider for advice on gaining weight.`;
  } else if (bmi >= 18.5 && bmi < 24.9) {
    suggestion = "You have a healthy weight. Maintain a balanced diet and regular exercise.";
  } else if (bmi >= 25 && bmi < 29.9) {
    suggestion = "You are overweight. Consider adopting a healthier diet and increasing your physical activity.";
  } else {
    suggestion = "You are obese. It's advisable to consult a healthcare provider for personalized advice.";
  }

  // Add the greeting based on gender
  if (gender === "male") {
    return `Dear Sir ${firstname} ${lastname}, ${suggestion}`;
  } else if (gender === "female") {
    return `Dear Madam ${firstname} ${lastname}, ${suggestion}`;
  } else {
    return `Hello ${firstname} ${lastname}, ${suggestion}`;
  }
};


</script>

<template>
  <div class="container mx-auto p-4 mt-5">
    <!-- BMI Calculator -->
    <div class="bg-green-100 p-4 rounded-lg shadow-lg mt-6 flex items-center space-x-4" v-if="BodyMassIndex !== null">
      <i class="fa-solid fa-circle-check text-green-600 text-2xl"></i>
      <div>
        <h4 class="text-green-700 text-xl font-semibold mb-1">BMI Calculated Successfully!</h4>
        <p class="text-green-800 text-lg">Your Body Mass Index (BMI) is: <strong>{{ BodyMassIndex.toFixed(2) }}</strong></p>
      </div>
    </div>

    <strong class="text-2xl font-weight-bold">Body Mass Index ( BMI ) Calculator</strong>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <!-- Credentials Form -->
      <div class="bg-white p-6 rounded-lg shadow-lg">
        <h4 class="text-xl font-bold mb-4">Credentials Form</h4>

        <form method="post" @submit.prevent="submitCredentialsForm">
          <div class="mb-4">
            <label for="firstname" class="block text-gray-700 font-semibold">First Name:</label>
            <input
              type="text"
              id="firstname"
              name="firstname"
              required
              v-model="firstname"
              class="w-full p-2 border border-gray-300 rounded mt-2 focus:outline-none focus:border-blue-500" />
          </div>

          <div class="mb-4">
            <label for="lastname" class="block text-gray-700 font-semibold">Last Name:</label>
            <input
              type="text"
              id="lastname"
              name="lastname"
              required
              v-model="lastname"
              class="w-full p-2 border border-gray-300 rounded mt-2 focus:outline-none focus:border-blue-500" />
          </div>
          <div class="mb-4">
            <label for="gender" class="block text-gray-700 font-semibold">Gender:</label>
            <select
              type="select"
              id="gender"
              name="gender"
              required
              v-model="gender"
              class="w-full p-2 border border-gray-300 rounded mt-2 focus:outline-none focus:border-blue-500" >
              <option value="">-- Select Gender --</option>
              <option value="male">Male</option>
              <option value="female">Female</option>
            </select>
          </div>
          <button
            type="submit"
            class="w-full bg-blue-500 text-white font-semibold py-2 px-4 rounded hover:bg-blue-600">
            <i class="fas fa-paper-plane"></i>
            Submit
          </button>
        </form>
      </div>

      <!-- Body Readings Form -->
      <div
        class="bg-white p-6 rounded-lg shadow-lg"
        v-if="isCredentialFormCompleted">
        <h4 class="text-xl font-bold mb-4">Body Readings Form</h4>
        <form @submit.prevent="submitBodyReadings">
          <div class="mb-4">
            <label for="height" class="block text-gray-700 font-semibold">Height (in meters):</label>
            <input
              type="number"
              step="0.01"
              id="height"
              name="height"
              v-model="height"
              placeholder="Enter your height"
              class="w-full p-2 border border-gray-300 rounded mt-2 focus:outline-none focus:border-blue-500" />
          </div>

          <div class="mb-4">
            <label for="weight" class="block text-gray-700 font-semibold">Weight (in kg):</label>
            <input
              type="number"
              id="weight"
              name="weight"
              v-model="weight"
              placeholder="Enter your weight"
              class="w-full p-2 border border-gray-300 rounded mt-2 focus:outline-none focus:border-blue-500" />
          </div>

          <button
            type="submit"
            class="w-full bg-blue-500 text-white font-semibold py-2 px-4 rounded hover:bg-blue-600">
            <i class="fas fa-paper-plane"></i>
            Submit
          </button>
        </form>
      </div>

      <!-- Warning Alert if Credentials Not Completed -->
      <div v-else>
        <div
          class="bg-yellow-100 border-l-4 border-yellow-500 text-yellow-700 p-4 rounded-lg flex items-center mt-4"
          role="alert">
          <i class="fa-solid fa-circle-exclamation text-yellow-500 text-2xl mr-3"></i>
          <div>
            <p class="font-bold">Warning</p>
            <p>
              This is a warning message. Please fill in the Credentials first, then proceed.
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- Suggestions Card -->
    <div class="mt-6" v-if="BodyMassIndex !== null">
      <div class="bg-blue-100 border-l-4 border-blue-500 text-blue-700 p-4 rounded-lg">
        <h4 class="font-bold">Suggestions Based on Your BMI</h4>
        <p>{{ getBMISuggestions(BodyMassIndex, firstname, lastname,gender) }}</p>
      </div>
    </div>

  </div>
</template>
