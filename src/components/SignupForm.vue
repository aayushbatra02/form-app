<template>
  <div
    class="border border-8 border-grey p-6 m-4 rounded md:w-[60%] lg:w-[40%] xl:w-[35%] md:m-auto md:mt-8"
  >
    <div class="mb-4 sm:mb-5 md:mb-6 lg:mb-7 xl:mb-8">
      <label class="text-gray-400 font-bold block" for="email">EMAIL:</label>
      <input
        v-model.trim="email"
        class="border border-grey my-2 px-3 py-2 w-[100%]"
        type="email"
      />
      <div v-if="emailErrorMessage" class="text-red-700">
        {{ emailErrorMessage }}
      </div>
    </div>
    <div class="mb-4 sm:mb-5 md:mb-6 lg:mb-7 xl:mb-8">
      <label class="text-gray-400 font-bold block" for="password"
        >PASSWORD:</label
      >
      <input
        v-model="password"
        class="border border-grey my-2 px-3 py-2 w-[100%]"
        type="password"
      />
      <div v-if="passwordErrorMessage" class="text-red-700">
        {{ passwordErrorMessage }}
      </div>
    </div>
    <div class="mb-4 sm:mb-5 md:mb-6 lg:mb-7 xl:mb-8">
      <label class="text-gray-400 font-bold block" for="Role">ROLE:</label>
      <select
        v-model="role"
        class="border border-grey my-2 px-3 py-2 w-[100%] text-gray-500"
      >
        <option>Web Developer</option>
        <option>Web Designer</option>
      </select>
    </div>
    <div>
      <label class="text-gray-400 font-bold block" for="skills">SKILLS:</label>
      <input
        v-model.trim="skillsInput"
        @keyup="addSkill"
        class="border border-grey my-2 px-3 py-2 w-[100%]"
        type="text"
      />
      <div v-if="skillErrorMessage" class="text-red-700">
        {{ skillErrorMessage }}
      </div>
    </div>

    <div
      class="mb-4 sm:mb-5 md:mb-6 lg:mb-7 xl:mb-8 flex flex-row flex-wrap gap-4 mt-2"
    >
      <button
        @click.self="editSkill(id)"
        v-for="(skill, id) in skills"
        class="bg-gray-200 px-4 py-1 rounded-2xl text-gray-500 font-bold flex gap-2"
        :key="id"
      >
        {{ skill }}
        <button @click="deleteSkill(id)" class="text-red-500">x</button>
      </button>
    </div>
    <div class="mb-2 flex gap-2">
      <input
        v-model="isTermsAndConditionedChecked"
        class="cursor-pointer w-4 h-4"
        id="checkbox"
        type="checkbox"
      />
      <label class="text-gray-400 font-bold text-xs lg:text-sm" for="checkbox"
        >ACCEPT TERMS AND CONDITIONS</label
      >
    </div>
    <div v-if="termsAndConditionErrorMessage" class="text-red-700">
      {{ termsAndConditionErrorMessage }}
    </div>
    <div class="mt-8 xl:mt-12 flex justify-center">
      <button
        @click="validateForm"
        class="bg-blue-500 py-2 px-4 rounded-[3rem] text-white"
      >
        Create an Account
      </button>
    </div>
  </div>
  <account-details
    v-if="isAccountCreated"
    :accountDetails="accountDetails"
  ></account-details>
</template>

<script>
import AccountDetails from "./AccountDetails";

export default {
  components: { AccountDetails },
  data() {
    return {
      email: "null@sss.ddd",
      password: "ss@34R5zxcds",
      role: "Web Developer",
      skillsInput: "",
      skills: ["test"],
      isTermsAndConditionedChecked: true,
      editIndex: null,
      emailErrorMessage: null,
      passwordErrorMessage: null,
      skillErrorMessage: null,
      termsAndConditionErrorMessage: null,
      isAccountCreated: false,
      accountDetails: null,
    };
  },
  methods: {
    addSkill(e) {
      if (this.skillsInput.includes(",")) {
        let skill = this.skillsInput.split("");
        skill = skill.filter((char) => char !== ",");
        this.skillsInput = skill.join("");
      }
      if (this.skillsInput !== "" && this.skillsInput !== ",") {
        if (e.key === "Enter" || e.key === ",") {
          if (this.editIndex || this.editIndex === 0) {
            this.skills[this.editIndex] = this.skillsInput;
          } else {
            this.skills.push(this.skillsInput);
          }
          this.skillsInput = "";
          this.editIndex = null;
        }
      }
    },
    deleteSkill(id) {
      this.skills = this.skills.filter((skill, index) => index !== id);
    },
    editSkill(id) {
      this.skillsInput = this.skills[id];
      this.editIndex = id;
    },
    validateForm() {
      const validateEmail = (email) => {
        return String(email)
          .toLowerCase()
          .match(
            /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          );
      };

      function validatePassword(str) {
        var re = /^(?=.*\d)(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z]).{8,}$/;
        return re.test(str);
      }

      if (!this.email) {
        this.emailErrorMessage = "Email is required";
      } else if (!validateEmail(this.email)) {
        this.emailErrorMessage = "Please enter a valid email";
      } else {
        this.emailErrorMessage = null;
      }

      if (!this.password) {
        this.passwordErrorMessage = "Password is required";
      } else if (!validatePassword(this.password)) {
        this.passwordErrorMessage = `min 8 letters, at least a symbol, upper and lower case letters and a number`;
      } else {
        this.passwordErrorMessage = null;
      }

      if (this.skills.length === 0) {
        this.skillErrorMessage = "Please add minimum one skill";
      } else {
        this.skillErrorMessage = null;
      }

      if (!this.isTermsAndConditionedChecked) {
        this.termsAndConditionErrorMessage =
          "Please accept Terms and Conditions";
      } else {
        this.termsAndConditionErrorMessage = null;
      }

      if (
        !this.emailErrorMessage &&
        !this.passwordErrorMessage &&
        !this.skillErrorMessage &&
        !this.termsAndConditionErrorMessage
      ) {
        this.isAccountCreated = true;
        this.accountDetails = {
          email: this.email,
          password: this.password,
          role: this.role,
          skills: this.skills,
        };
      }
    },
  },
};
</script>