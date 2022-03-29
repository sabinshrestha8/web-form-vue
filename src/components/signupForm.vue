<template>
    <!-- Attaching event modifier to submit event that prevents the
    default action of form being submitted i.e reload the page -->
    <form @submit.prevent="handleSubmit">
        <label>Email:</label>
        <!-- using directive v-model to sync form field and data property i.e. email together -->
        <input type="email" required v-model="email" />

        <label>Password:</label>
        <input type="password" required v-model="password" />
        <!-- output error if only passwordError has value and not '' -->
        <div v-if="passwordError" class="error">
            {{ passwordError }}
        </div>

        <label>Role:</label>
        <select v-model="role">
            <option value="developer">Web Developer</option>
            <option value="designer">Web Designer</option>
        </select>

        <label>Skills:</label>
        <input type="text" v-model="tempSkill" @keyup.alt="addSkill">
        <!-- key should be unique for each skill -->
        <div v-for="skill in skills" :key="skill" class="pill">
            <!-- we have access to each skill bcuz we are in 
            v-for loop & we can pass the skill as an args -->
            <span @click="deleteSkill(skill)">{{ skill }}</span>
        </div>

        <!-- First way to work with checkboxes, by using single 
        checkbox with v-model that would be either true or 
        false dependent on whether it's checked or not -->
        <div class="terms">
            <input type="checkbox" v-model="terms" required />
            <label>Accept terms and conditions</label>
        </div>

        <div class="submit">
            <button>Create an account</button>
        </div>
    </form>

    <p>Email: {{ email }}</p>
    <p>Password: {{ password }}</p>
    <p>Role: {{ role }}</p>
    <p>Terms accepted: {{ terms }}</p>
</template>

<script>
export default {
    data() {
        return {
            /* when a user types in input, email property 
            gets updated with value that they type in */
            email: "mario",
            password: "",
            role: "",
            terms: false,
            tempSkill: "",
            skills: [],
            passwordError: ""
        }
    },
    methods: {
        addSkill(e) {
            // only gonna fire if tempskill has a value & we press comma
            if (e.key === "," && this.tempSkill) {
                /* check if the skills array alreay includes 
                tempSkill which we're trying to add to it 
                */
                if (!this.skills.includes(this.tempSkill)) {
                    this.skills.push(this.tempSkill);
                }
                // clear the input field
                this.tempSkill = "";
            }
        },
        // deletes the specific skill from the skill array but we need to know what skill to delete
        // takes in the skill as an args passed from the click event
        deleteSkill(skill) {
            // using filter method on the array to filter out selected skill
            this.skills = this.skills.filter((item) => {
                /* check does the skill we take in as parameter equals to item, 
                if it does then return false & remove that skill from the array */
                return skill != item
            })
        },
        handleSubmit() {
            // validate password
            this.passwordError = this.password.length > 5 ? 
                '' : 'password must be atleast 6 chars long'
    
            console.log('form submitted');

            if(!this.passwordError) {
                console.log('email:', this.email);
                console.log('password:', this.password);
                console.log('role:', this.role);
                console.log('skills:', this.skills);
                console.log('terms accepted:', this.terms);
            }
        }
    },
};
</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}

label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}

input,
select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}

input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}

.pill {
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}

button {
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
    cursor: pointer;
}

.submit {
    text-align: center;
}

.error {
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
</style>
