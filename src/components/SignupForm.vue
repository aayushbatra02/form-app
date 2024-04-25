<template>
  <div
    class="border border-8 border-grey p-6 2xl:p-8 m-4 rounded md:w-[60%] lg:w-[40%] xl:w-[35%] md:m-auto md:mt-8 2xl:mt-10"
  >
    <div class="mb-4">
      <label class="text-gray-400 font-bold block 2xl:text-lg" for="email"
        >EMAIL:</label
      >
      <input
        v-model.trim="formData.email"
        class="border border-grey my-2 px-3 py-2 w-[100%]"
        type="email"
        @input="checkEmail"
      />
      <div v-if="emailErrorMessage" class="text-red-700 sm:text-base">
        {{ emailErrorMessage }}
      </div>
    </div>
    <div class="mb-4">
      <label class="text-gray-400 font-bold block 2xl:text-lg" for="password"
        >PASSWORD:</label
      >
      <input
        v-model="formData.password"
        class="border border-grey my-2 px-3 py-2 w-[100%]"
        type="password"
        @input="checkPassword"
      />
      <div v-if="passwordErrorMessage" class="text-red-700 sm:text-base">
        {{ passwordErrorMessage }}
      </div>
    </div>
    <div class="mb-4">
      <label class="text-gray-400 font-bold block 2xl:text-lg" for="Role"
        >ROLE:</label
      >
      <select
        v-model="formData.role"
        class="border border-grey my-2 px-3 py-2 w-[100%]"
        @change="checkRoles"
      >
        <option disabled>Select a Role</option>
        <option value="Web Developer">Web Developer</option>
        <option value="Web Designer">Web Designer</option>
      </select>
      <div
        v-if="roleErrorMessage"
        class="text-red-700 sm:text-base sm:text-base"
      >
        {{ roleErrorMessage }}
      </div>
    </div>
    <div>
      <label class="text-gray-400 font-bold block 2xl:text-lg" for="skills"
        >SKILLS:</label
      >
      <input
        v-model.trim="formData.skillsInput"
        @keyup="addSkill"
        class="border border-grey my-2 px-3 py-2 w-[100%]"
        type="text"
        ref="skillsInputField"
      />
      <div
        v-if="skillErrorMessage"
        class="text-red-700 sm:text-base sm:text-base"
      >
        {{ skillErrorMessage }}
      </div>
    </div>
    <div class="mb-4 flex flex-row flex-wrap gap-4 mt-2">
      <button
        @click.self="editSkill(id)"
        v-for="(skill, id) in formData.skills"
        class="bg-gray-200 px-4 py-1 rounded-2xl text-gray-500 font-bold flex gap-2"
        :key="id"
      >
        {{ skill }}
        <button @click="deleteSkill(id)" class="text-red-500">x</button>
      </button>
    </div>
    <div class="mb-2 flex gap-4 items:center">
      <input
        v-model="formData.isTermsAndConditionedChecked"
        class="cursor-pointer w-4 h-4 2xl:w-5 2xl:h-5"
        id="checkbox"
        type="checkbox"
        @change="validateTermsAndCondition"
        ref="skillsInput"
      />
      <label
        class="text-gray-400 font-bold text-xs lg: sm:text-base"
        for="checkbox"
        >ACCEPT TERMS AND CONDITIONS</label
      >
    </div>
    <div
      v-if="termsAndConditionErrorMessage"
      class="text-red-700 sm:text-base sm:text-base"
    >
      {{ termsAndConditionErrorMessage }}
    </div>
    <div class="mt-8 xl:mt-10 flex justify-center">
      <button
        @click="validateForm"
        class="bg-blue-500 py-2 2xl:py-3 px-4 2xl:px-6 rounded-[3rem] text-white"
      >
        Create an Account
      </button>
    </div>
  </div>
  <accountDetails v-if="isAccountCreated" :accountDetails="accountDetails" />
</template>

<script>
import AccountDetails from "./AccountDetails";

export default {
  components: { AccountDetails },
  data() {
    return {
      formData: {
        email: null,
        password: null,
        role: "Select a Role",
        skillsInput: "",
        isTermsAndConditionedChecked: false,
        skills: [],
      },
      skillEditIndex: null,
      formValidation: false,
      emailErrorMessage: null,
      passwordErrorMessage: null,
      skillErrorMessage: null,
      roleErrorMessage: null,
      termsAndConditionErrorMessage: null,
      isAccountCreated: false,
      accountDetails: {
        email: null,
        password: null,
        role: null,
        skills: null,
      },
    };
  },
  methods: {
    addSkill(e) {
      if (this.formData.skillsInput.includes(",")) {
        let skill = this.formData.skillsInput.split("");
        skill = skill.filter((char) => char !== ",");
        this.formData.skillsInput = skill.join("");
      }
      if (
        this.formData.skillsInput !== "" &&
        this.formData.skillsInput !== ","
      ) {
        if (e.key === "Enter" || e.key === ",") {
          if (this.skillEditIndex || this.skillEditIndex === 0) {
            this.formData.skills[this.skillEditIndex] =
              this.formData.skillsInput;
          } else {
            this.formData.skills.push(this.formData.skillsInput);
          }
          this.formData.skillsInput = "";
          this.skillEditIndex = null;
        }
      }
      this.checkSkills();
    },
    deleteSkill(id) {
      this.formData.skills = this.formData.skills.filter(
        (skill, index) => index !== id
      );
      if (this.skillEditIndex || this.skillEditIndex === 0) {
        this.formData.skillsInput = "";
        this.skillEditIndex = null;
      }
      this.checkSkills();
    },
    editSkill(id) {
      this.formData.skillsInput = this.formData.skills[id];
      this.skillEditIndex = id;
      this.$refs.skillsInputField.focus();
    },
    validateEmail(email) {
      let regex =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return regex.test(email);
    },
    validatePassword(password) {
      let regex = /^(?=.*\d)(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z]).{8,}$/;
      return regex.test(password);
    },
    checkEmail() {
      if (this.formValidation) {
        if (!this.formData.email) {
          this.emailErrorMessage = "Email is required";
        } else if (!this.validateEmail(this.formData.email)) {
          this.emailErrorMessage = "Please enter a valid email";
        } else {
          this.emailErrorMessage = null;
        }
      }
    },
    checkPassword() {
      if (this.formValidation) {
        if (!this.formData.password) {
          this.passwordErrorMessage = "Password is required";
        } else if (!this.validatePassword(this.formData.password)) {
          this.passwordErrorMessage = `min 8 letters, at least a special character, upper and lower case letters and a number`;
        } else {
          this.passwordErrorMessage = null;
        }
      }
    },
    checkSkills() {
      if (this.formValidation) {
        if (this.formData.skills.length === 0) {
          this.skillErrorMessage = "Please add minimum one skill";
        } else {
          this.skillErrorMessage = null;
        }
      }
    },
    checkRoles() {
      if (this.formValidation) {
        if (this.formData.role === "Select a Role") {
          this.roleErrorMessage = "Please select a role";
        } else {
          this.roleErrorMessage = null;
        }
      }
    },
    validateTermsAndCondition() {
      if (this.formValidation) {
        if (!this.formData.isTermsAndConditionedChecked) {
          this.termsAndConditionErrorMessage =
            "Please accept Terms and Conditions";
        } else {
          this.termsAndConditionErrorMessage = null;
        }
      }
    },
    validateForm() {
      this.formValidation = true;
      this.checkEmail();
      this.checkPassword();
      this.checkSkills();
      this.checkRoles();
      this.validateTermsAndCondition();
      if (
        !this.emailErrorMessage &&
        !this.passwordErrorMessage &&
        !this.skillErrorMessage &&
        !this.termsAndConditionErrorMessage &&
        !this.roleErrorMessage
      ) {
        this.isAccountCreated = true;
        for (const key in this.accountDetails) {
          this.accountDetails[key] = this.formData[key];
        }
        this.formData.email = null;
        this.formData.password = null;
        this.formData.role = "Select a Role";
        this.formData.skills = [];
        this.formData.isTermsAndConditionedChecked = false;
      }
    },
  },
};
</script>