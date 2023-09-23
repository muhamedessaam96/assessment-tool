<template>
  <div class="main-page">
    <!-- <gauge-chart /> -->
    <div class="assessment-container">
      <img src="../src/assets/logoweb.png" class="logo-img" />
      <h2
        style="margin-bottom: 20px; text-align: center; font-size: 20px"
        v-if="step === 1"
      >
        Wellness Readiness®
      </h2>
      <div class="step-intro" style="padding: 10px;" key="step-intro" v-if="step === 1">
        <p style="
            font-weight: 400;
            font-size: 16px;
            word-spacing: 0px;">
        Thank you for taking the Organisation Wellness Assessment by TTM Associates.
        </p>
        <p style=" font-weight: 400;">This assessment will provide you with a snapshot of the Employee Wellness inside your organisation , a very critical elements for an organisation to 
        thrive in the new era!</p>
        <p>The Assessment entails 8 questions.</p>
        <p>Please ensure that your responses reflect the current state in your organisation.</p>
        <p>Enjoy!</p>
        <p>The TTM Team </p>
        <div class="terms_conditions" >
          <input type="checkbox" id="termsAgree" name="terms_conditions" @click="termsAgree =!termsAgree"/>
          <label for="termsAgree" style="font-weight: 500; ">I agree with </label>
          <a href="#">
            <terms-and-conditions style="width: 100%; "/>
          </a>
        </div>
        <p style="color: red" v-if="showTermsAgree">
          Please agree with terms and conditions in order to continue
        </p>
      </div>

      <div class="step-question" v-else-if="step > 1 && step <= 9" :key="step">
        <p style="text-align: center; font-weight: bold;"> Employee Wellness® Readiness</p>
        <h3 style="text-align: center; color: #616161; font-size: 16px; margin-top: 0; padding-bottom: 30px;">
          Question {{ step - 1 }}/8
          <hr style="height: 2px; background-color: #616161; width: 100px; border-radius: 100%;">
        </h3>
        
        <p style="font-weight: 700;margin-bottom: 1%;">{{ questions[step - 2].text }}.</p>
        <div class="answers">
          <label
            class="answer-label"
            v-for="answer in questions[step - 2].answers"
            :key="answer.score"
            :style="[
              answer.selected ? 'color: white;  background-color: #5d2977' : '',
            ]"
          >
            <input
              class="answer-radio"
              type="radio"
              :name="`question${step - 1}`"
              :value="answer.score"
              v-model="answers[step - 2]"
              @change="updateAnswerState(step - 2, answer)"
            />
            <span class="checkbox-text">{{ answer.text }}</span>
            <span
              class="checkmark"
              :class="{ checked: answer.selected }"
            ></span>
          </label>
        </div>
      </div>

      <div
        class="step-user-details"
        v-else-if="step === 10"
        key="step-user-details"
        style="margin-top: 30px"
      >
        <p style="font-size: 16px">
          Please complete the form to share with you the report result
        </p>
        <form @submit.prevent="submitForm">
          <div class="form-row">
            <label>
              <span>Name:</span>
              <input
                class="user-input"
                type="text"
                v-model="userName"
                required
                placeholder="user name"
              />
            </label>
            <label>
              <span>Surname:</span>
              <input
                class="user-input"
                type="text"
                v-model="userSurname"
                required
                placeholder="Surname"
              />
            </label>
          </div>
          <!-- <div class="form-row">
            <label>
              <span>Email:</span>
              <input
                class="user-input"
                type="email"
                v-model="userEmail"
                required
                placeholder="Email"
              />
            </label>
            <label>
              <span>Mobile Number:</span>
              <input
                class="user-input"
                type="tel"
                v-model="userMobile"
                required
                placeholder="Mobile Number"
              />
            </label>
          </div> -->
          <div class="form-row" style="display: flex;">
         <div style="display: flex; flex-direction: column ; width: 50%;">

          <label>
            <div style="display: flex; align-items: center; gap: 2px;">
              <span>Email:</span>
              <span class="note" style="font-size: 10px;color: red;">Use your business Email</span>
              <!-- <div class="form-row tooltip" style="padding-top: 5px;" data-tooltip="insert you business email for exclusive content">
                 <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="14.899"
                    height="14.899"
                    viewBox="0 0 14.899 14.899"
                >
                    <g id="info_2_" data-name="info (2)" opacity="0.5">
                        <path
                            id="Path_25877"
                            data-name="Path 25877"
                            d="M7.45,0A7.45,7.45,0,1,0,14.9,7.45,7.45,7.45,0,0,0,7.45,0Zm0,13.658A6.208,6.208,0,1,1,13.658,7.45,6.208,6.208,0,0,1,7.45,13.658Z"
                            transform="translate(0 0)"
                        />
                        <path
                            id="Path_25878"
                            data-name="Path 25878"
                            d="M11.339,10h-.67a.67.67,0,1,0,0,1.339h.67v4.018a.67.67,0,0,0,1.339,0V11.339A1.339,1.339,0,0,0,11.339,10Z"
                            transform="translate(-3.89 -4.085)"
                        />
                        <ellipse
                            id="Ellipse_1216"
                            data-name="Ellipse 1216"
                            cx="1.005"
                            cy="1.005"
                            rx="1.005"
                            ry="1.005"
                            transform="translate(6.445 3.069)"
                        />
                    </g>
                </svg>
              </div> -->
            </div>
              <input
                class="user-input"
                type="email"
                v-model="userEmail"
                required
                placeholder="Email"
              />
            </label>
            <span class="error-message">{{ emailError }}</span>
         </div>
         <div style="display: flex; flex-direction: column ; width: 50%;">
            <label>
              <span>Mobile Number:</span>
              <input
                class="user-input"
                type="tel"
                v-model="userMobile"
                required
                placeholder="Example:+966xxx"
              />
            </label>
            <span class="error-message">{{ mobileError }}</span>
            </div>
          </div>
          <div class="form-row">
            <label>
              <span>Industry:</span>
              <input
                class="user-input"
                type="text"
                v-model="userIndustry"
                required
                placeholder="Industry"
              />
            </label>
            <label>
              <span>Company:</span>
              <input
                class="user-input"
                type="text"
                v-model="userCompany"
                required
                placeholder="Company"
              />
            </label>
          </div>
          <div class="form-row">
            <label>
              <span>Title:</span>
              <input
                class="user-input"
                type="text"
                v-model="userTitle"
                required
                placeholder="Title"
              />
            </label>
            <label>
              <span>Country:</span>
              <input
                class="user-input"
                type="text"
                v-model="userCountry"
                required
                placeholder="Country"
              />
            </label>
          </div>
          <!-- <button class="submit-button" type="submit">Submit</button> -->
        </form>
      </div>

      <div class="step-result" v-else-if="step > 10" key="step-result">
        <!-- <h2>Result</h2> -->
          <div class="step-result-chart">
            <div class="top-result" >
              <img src="./assets/high.png"  v-if="socore>26"/>
              <img src="./assets/medium.png"  v-if="socore>13 &&socore<26"/>
              <img src="./assets/lowtest.png"  v-if="socore<13"/>
             <p style=" margin: 0;">Your score: {{ calculateScore() }} out of 40</p>
           </div>
            <div class="right-step-result-chart" v-if="socore>26">
              <h4 style="font-size: 14px;font-weight: bold;">Organisation Wellness</h4>
                

              <p style="font-size: 14px; font-weight: normal;">Good position in your <span style="font-weight: bold;">Organisation Wellness</span> with high potential to thrive!</p>
                <p style="font-size: 12px;">Key steps have been taken to enhance <span style="font-weight: bold;">Employee Wellness</span> in your organisation for the new era.
                   However, it is important to accurately identify the <span style="font-weight: bold;">Wellness</span> level in your organisation immediately to formulate
                    appropriate plans that will enable the entire organisation to be aligned correctly and effectively, thus enhancing its potential to thrive in the new era.
                     A more detailed and focused <span style="font-weight: bold;">Organisational Wellness</span> approach is recommended, tailored to your organisation’s needs,
                      to identify the appropriate values within the organisation that can drive all departments to achieve their goals and enhance employee loyalty
                       and engagement while increasing workforce performance, thus enabling the organisation to achieve its goals.
                </p>
            </div>
            <div class="right-step-result-chart" v-if="socore>13 &&socore<26">
              <h4 style="font-size: 14px;font-weight: bold;">Organisation Wellness</h4>
              <p style="font-size: 14px; font-weight: normal;">Average position in your <span style="font-weight: bold;">Organisation Wellness</span> with promising growth potential!</p>
                <p style="font-size: 12px;">
                  To enhance <span style="font-weight: bold;">Employee Wellness</span> in your organisation, it is critical to implement impactful enhancements and enable <span style="font-weight: bold;">Wellness</span> across the entire organisation.
                   Identify the right values within the organisation that can drive all functions to enhance employee loyalty and engagement while increasing workforce performance.
                    A more focused <span style="font-weight: bold;">Organisational Wellness</span> approach is needed,
                   tailored to the needs of your organisation and supported by all internal functions and employees to achieve business goals.
                </p>
            </div>
            <div class="right-step-result-chart" v-if="socore<=13">
              <h4 style="font-size: 14px;font-weight: bold;">Organisation Wellness</h4>
               <p style="font-size: 14px; font-weight: normal;">Vulnerable position in your <span style="font-weight: bold;">Organisation Wellness</span> with substantial room for improvement!</p>
               <p style="font-size: 12px;">In the new environment, it is critical to consider realigning <span style="font-weight: bold;">Organisational Wellness</span> to be driven by your key values,
              which will guide all areas within your organisation to substantially enhance employee loyalty, engagement, and workforce performance.
                It is critical to identify the wellness within your organisation as soon as possible,
                identify the critical gap, and formulate business plans accordingly</p>
            </div>
          </div>
          <!-- <div class="bottom-result-step">
            <div class="bottom-result-step-text" style=" color: black; width: 40%; height: 180px; padding: 10px;">
              <h4 style="font-size: 14px;font-weight: bold; color: #5d2977;">Organisation Wellness</h4>
              <p style="color: black; font-size: 14px;" >We empower organisations with clear insights to build a solid foundation of optimal Organisation
                  Wellness where leadership executives Lead Well, managerial suites Manage Well and staff professionals Do 
                  Well.</p>
            </div>
            <div class="bottom-result-step-img" style="width: auto; background-color: rgb(199, 199, 199);"><img src="./assets/1.jpeg" alt="" style="width: 210px; height:200px; "></div>
          </div> -->
      </div>

      <div class="buttons">
        <button v-if="step === 1" @click="startStep()" class="start-btn">
          Start
        </button>
        <button v-if="step > 1 && step <= 9" @click="prevStep" class="back-btn">
          Back
        </button>
        <button v-if="step < 10 && step > 1" @click="nextStep" class="next-btn">
          Next
        </button>
        <button
          class="submit-button"
          type="submit"
          @click="submitForm"
          v-else-if="step === 10"
        >
          Submit
        </button>
        <button  class="result-button" v-if="step==11 && !isPDf"  @click="downloadPDF"
             style=" width: 100%; background-color: red; border: none; color: white; padding:10px ;">Download PDF</button>
      </div>
      <div v-if="showError" class="modal">
      <div class="modal-content-in">
        <!-- <span class="close" >&times;</span> -->
        <p style="color: gray; font-weight: 500;">Info Message</p>
        <hr>
        <p>You must select an answer to proceed</p>
        <hr>
        <div style="display: flex; justify-content: end;">
          <button @click="showError = false" style="border-radius: 5px; background-color: rgb(158, 17, 22); color: white; padding: 8px; border: none;">close</button>
        </div>
      </div>
    </div>
    </div>
  </div>
</template>
<script>
import TermsAndConditions from './components/TermsAndConditions.vue';
import html2pdf from 'html2pdf.js';
import Email from '@/assets/smtp/smtp.js';
import axios from 'axios';


export default {
  components: { TermsAndConditions},
  data() {
    return {
      showError: false,
      step: 1,
      assessment: {
        title: "Wellness Assessment",
      },
      questions: [
        {
          text: "I feel good about the condition of my body",
          answers: [
            { text: "Strongly Agree", score: 5, selected: false },
            { text: "Agree", score: 4, selected: false },
            { text: "Do not Agree or Disagree", score: 3, selected: false },
            { text: "Disagree", score: 2, selected: false },
            { text: "Strongly Disagree", score: 1, selected: false },
          ],
        },
        {
          text: "I recognise the ways stress signals show up in my body  because of my work",
          answers: [
            { text: "Strongly Agree", score: 5, selected: false },
            { text: "Agree", score: 4, selected: false },
            { text: "Do not Agree or Disagree", score: 3, selected: false },
            { text: "Disagree", score: 2, selected: false },
            { text: "Strongly Disagree", score: 1, selected: false },
          ],
        },
        {
          text: "I feel good about myself and that others like me for who I am",
          answers: [
            { text: "Strongly Agree", score: 5, selected: false },
            { text: "Agree", score: 4, selected: false },
            { text: "Do not Agree or Disagree", score: 3, selected: false },
            { text: "Disagree", score: 2, selected: false },
            { text: "Strongly Disagree", score: 1, selected: false },
          ],
        },
        {
          text: "I can openly express my feelings without being judged",
          answers: [
            { text: "Strongly Agree", score: 5, selected: false },
            { text: "Agree", score: 4, selected: false },
            { text: "Do not Agree or Disagree", score: 3, selected: false },
            { text: "Disagree", score: 2, selected: false },
            { text: "Strongly Disagree", score: 1, selected: false },
          ],
        },
        {
          text: "I take time alone to think about what's important in life - who I am, what I value, where I fit in, and where I'm going",
          answers: [
            { text: "Strongly Agree", score: 5, selected: false },
            { text: "Agree", score: 4, selected: false },
            { text: "Do not Agree or Disagree", score: 3, selected: false },
            { text: "Disagree", score: 2, selected: false },
            { text: "Strongly Disagree", score: 1, selected: false },
          ],
        },
        {
          text: "I engage in acts of kindness and good will without expecting anything in return",
          answers: [
            { text: "Strongly Agree", score: 5, selected: false },
            { text: "Agree", score: 4, selected: false },
            { text: "Do not Agree or Disagree", score: 3, selected: false },
            { text: "Disagree", score: 2, selected: false },
            { text: "Strongly Disagree", score: 1, selected: false },
          ],
        },
        {
          text: "I learn from my mistakes and integrate those learnings in the future",
          answers: [
            { text: "Strongly Agree", score: 5, selected: false },
            { text: "Agree", score: 4, selected: false },
            { text: "Do not Agree or Disagree", score: 3, selected: false },
            { text: "Disagree", score: 2, selected: false },
            { text: "Strongly Disagree", score: 1, selected: false },
          ],
        },
        {
          text: "I am aware of my own feelings, and perceptions when responding to my work-life’s challenges",
          answers: [
            { text: "Strongly Agree", score: 5, selected: false },
            { text: "Agree", score: 4, selected: false },
            { text: "Do not Agree or Disagree", score: 3, selected: false },
            { text: "Disagree", score: 2, selected: false },
            { text: "Strongly Disagree", score: 1, selected: false },
          ],
        },
        // Add more questions here...
      ],
      answers: [],
      userName: "",
      userSurname: "",
      userEmail: "",
      userMobile: "",
      userIndustry: "",
      userCompany: "",
      userTitle: "",
      userCountry: "",
      Assessment_Name: "Wellness Assessment",
      termsAgree: false,
      showTermsAgree:false,
      socore:null,
      emailError:'',
      mobileError:'',
      isPDf:false
    };
  },
  created() {
    setInterval(() => (this.toggle = !this.toggle), 1111);
  },
 async mounted(){
  await  this.makeApiCalltest()
  },
  methods: {
    sendEmail() {
  // ... Your existing code for sending the email ...
  this.isPDf = true;
  // Generate the PDF
  const element = document.querySelector('.step-result');
  const options = {
    margin: [0, 0, 0, 0],
    filename: 'Report.pdf',
    image: { type: 'jpeg', quality: 0.98 },
    html2canvas: { scale: 2 },
    jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
  };

  html2pdf().from(element).set(options).output('datauristring').then((pdfDataUri) => {
    // Attach the PDF to the email
    Email.send({
      Host: "smtp.elasticemail.com",
      Username: "muhamedessam96@hotmail.com",
      Password: "03E3C777907DAA6389E2840E4A0DD03B7782",
      To: this.userEmail,
      From: "muhamedessam96@hotmail.com",
      Subject: "The REPORT",
      Body: `Dear ${this.userName}, <br/><br/><br/>
             Thank you for completing the Organisation Wellness Assessment by TTM Associates.
             Kindly find below results for your assessment,
             <br/>This score is simply a measurement of where your
             organisation’s currently stands for the new era.
             We encourage you to book a free consultation with one of our consultants to gain a comprehensive
             understanding of your score, as well as explore the options to further enhance your organisation’s Wellness`,
      Attachments: [
        {
          name: "Report.pdf",
          data: pdfDataUri
        }
      ]
    }).then(
      message => alert(message)
    );
    this.isPDf = false;

  });

},
  // sendEmail() {
  //             Email.send({
  //             Host : "smtp.elasticemail.com",
  //             Username : "muhamedessam96@hotmail.com",
  //             Password : "03E3C777907DAA6389E2840E4A0DD03B7782",
  //             To : this.userEmail,
  //             From : "muhamedessam96@hotmail.com",
  //             Subject : "This REPORT",
  //             Body :`Dear ${this.userName}, <br/><br/><br/>
  //                   Thank you for completing the Organisation Wellness Assessment by TTM Associates.
  //                   Kindly find below results for your assessment,
  //                   <br/>This score is simply a measurement of where your
  //                   organisation’s currently stands for the new era.
  //                   We encourage you to book a free consultation with one of our consultants to gain a comprehensive
  //                   understanding of your score, as well as explore the options to further enhance your organisation’s Wellness`
  //       }).then(
  //         message => alert(message)
  //       );
  //   },
    downloadPDF() {
      this.isPDf = true;
      const element = document.querySelector('.main-page');
      const options = {
        margin: [0, 0, 0, 0],
        filename: 'Report.pdf',
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
      };

      html2pdf().from(element).set(options).save();
      // this.isPDf = false;

    },
    updateAnswerState(questionIndex, selectedAnswer) {
      this.questions[questionIndex].answers.forEach((answer) => {
        answer.selected = answer === selectedAnswer;
      });
    },
    startStep() {
      if (this.step === 1) {
        if (this.termsAgree == false) {
          this.showTermsAgree = true;
          return;
        }
        this.showTermsAgree = false;
        this.termsAgree = false;
        this.step++;
        return;
      }
    },
    nextStep() {
      if (this.step === 1) {
        this.step++;
        return;
      }
      if (this.step === 10) {
        return; // Prevent incrementing the step from the last step
      }
      if (this.answers[this.step - 2] === undefined) {
        this.showError = true;
          return;
      }

      this.step++;
    },

    prevStep() {
      this.step--;
    },
    calculateScore() {
      let score = 0;
      for (let answer of this.answers) {
        score += parseInt(answer);
      }
      this.socore =score
      return score;
    },
    submitForm() {
    this.emailError = "";
    this.mobileError = "";

    if (this.userName === "" || this.userEmail === "") {
      alert("Please enter your name and email.");
      return;
    }

    // Email validation
    const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailPattern.test(this.userEmail)) {
      this.emailError = "Please enter a valid email address.";
      return;
    }

    // Mobile number validation (assuming 10 digits for a basic example)
    const mobilePattern = /^\+\d{10}(?:\d{2})?$/;
    if (!mobilePattern.test(this.userMobile)) {
      this.mobileError = "Please enter a valid mobile number.";
      return;
}

    // If all validations pass, proceed to the next step
    this.step++;
  //  await this.sendEmail();
   this.makeApiCalltest();

  },
  
    async makeApiCalltest() {
   
            const apiUrl = 'https://www.zohoapis.eu/crm/v2/functions/assessmentresults/actions/execute?auth_type=apikey&zapikey=1003.fc485b68bdf73305340465df16b1ad49.ac8aaa1b4e66c4be0937ebc3e949f9e2';
            const params = {

              "Question_1": "Agree",

              "Question_2": "Agree",

              "Question_3": "Agree",

              "Question_4": "Agree",

              "Question_5": "Agree",

              "Question_6": "Agree",

              "Question_7": "Agree",

              "Question_8": "Agree",

              "Question_9": "",

              "Question_10": "",

              "Question_11": "",

              "Question_12": "",

              "Email": "fay.raftopoulou@repathsolutions.com",

              "Assessment_Name": "Wellness Assessment",

              "Company": "Company",

              "Country": "Country",

              "Industry": "Industry",

              "Job_Title": "Job_Title",

              "Mobile_Phone": "+30 6986724586",

              "First_Name": "First_Name",

              "Result": "5.0",

              "Score_Level": "High Level",

              "Surname": "Surname"

            };

            const formData = new FormData();

            formData.append("jsondata", JSON.stringify(params));
            try {
              const response = await axios.post(apiUrl, formData, {
              headers: {
                'Content-Type': 'multipart/form-data',
              }
            })
            console.log(error);
            } catch (error) {
              console.log('error' ,error);
            }
          
    },

  },
};
</script>

<style>
body {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
    }
    .tooltip {
    position: relative;
  }

  .tooltip::after {
    content: attr(data-tooltip);
    display: none;
    position: absolute;
    bottom: -5px;
    left: 60px;
    transform: translateX(-50%);
    padding: 5px;
    background-color: #fcfcfc;
    color: red;
    border-radius: 4px;
    white-space: nowrap;
    font-size: 14px;
  }

  .tooltip:hover::after {
    display: block;
  }
.result-button:hover{
      opacity: 0.8 !important;
    }

.top-result{
  background-color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 10px;
}
.top-result img{
  width: auto;
  height: 200px;
}
.bottom-result-step{
  height: 200px;
  background-image: url(./assets/4.jpeg);
  background-repeat: no-repeat;
  background-size: contain; 
  background-position: right;
}
.right-step-result-chart{
  background:#5d2977!important;
  /* background-image: url(./assets/4.jpeg);
  background-repeat: no-repeat;
  background-size: cover;  */
  color: white;
  width: auto;
  height: 50%;
  padding: 10px;
  /* height: 200px; */
}
.error-message{
  color: red;
  font-size: 14px;
}
.step-result-chart{
  display: flex;
  flex-direction: column;
  gap: 0;
}
.terms_conditions {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-items: center;
  gap: 10px;
  margin: 10px;
}
.terms_conditions input[type="checkbox"] {
  display: none;
}

.terms_conditions input[type="checkbox"] + label::before {
  content: "";
  display: inline-block;
  width: 16px;
  height: 16px;
  border: 1px solid #c5bfbf;
  border-radius: 3px;
  background-color: white;
  margin-right: 8px;
  vertical-align: middle;
}

.terms_conditions input[type="checkbox"]:checked + label::before {
  background-color: #5d2977;
}

.form-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.form-row label {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.form-row label span {
  font-weight: bold;
  margin-bottom: 5px;
}
.form-row label input {
  width: 90% !important;
}
.main-page {
  margin: 0;
  padding-top: 35px;
  padding-bottom: 35px;
  min-height: 100% !important;
  min-width: 100% !important;
  overflow: hidden !important;
  background-color: #5d2977;
}
.logo-img {
  display: flex;
  justify-content: center;
  /* align-items: center; */
  margin: auto;
  max-width: 25%;
  margin-top: 25px;
}
.answer-label {
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 5px;
}

.answer-radio {
  display: none;
}

.checkbox-text {
  margin-left: 10px;
}

.checkmark {
  position: relative;
  display: inline-block;
  width: 18px;
  height: 18px;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 50px;
  margin-right: 10px;
  cursor: pointer;
}

.checkmark::before {
  content: "\2713";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 12px;
  color: #5d2977;
  display: none;
}

.checked::before {
  display: block;
}

.answer-radio:checked + .checkmark::after {
  display: block;
}
.assessment-container {
  width: 650px;
  height: 650px;
  margin: 0 auto;
  padding-left: 20px;
  padding-right: 20px;
  padding-top:20px ;
  background-color: #efecec;
  border-radius: 0px;
}

.step-intro {
  text-align: left;
  background-color: white;
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
}
.step-result {
  background-color: #f8f8f8;
  margin-top: 20px;
  border-radius: 0px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  gap: 6px;
  height: auto;
}

.step-question {
  background-color: white;
  margin-top: 20px;
  border-radius: 0px;
  padding: 15px;
  /* margin-bottom: 20px; */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: max-content;
}
.step-user-details {
  background-color: #f1f1f1;
  margin-top: 20px;
  border-radius: 0px;
  padding: 15px;
  /* margin-bottom: 20px; */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.step-question h2,
.step-user-details h2,
.step-result h2 {
  margin-top: 0;
}

.answers {
  display: flex;
  flex-direction: column;
  margin-top: 10px;
}
:hover.answer-label {
  background-color: #5d2977;
  color: white;
}
.answer-label {
  margin-bottom: 5px;
}

.answer-radio {
  margin-right: 5px;
}

.step-user-details form {
  display: flex;
  flex-direction: column;
  background-color: #f1f1f1;
  padding-left: 4%;
}

.user-input {
  margin-bottom: 10px;
  padding: 5px;
  border-radius: 4px;
  /* border: 1px solid #ccc; */
  border: none;
  height:24px;
}

.submit-button {
  padding: 15px;
  background-color: #5d2977;
  color: #ffffff;
  border: none;
  cursor: pointer;
  width: 100%;
  height: auto;
}

.buttons {
  width: auto;
  display: flex;
  gap: 2px;
  justify-content: space-between;
  /* margin-top: 20px; */
}
.start-btn {
  width: 100% !important ;
  height: auto;
  padding: 15px;
  border: none;
  background-color: #6f318f;
  color: white;
  cursor: pointer;
}
.start-btn:hover {
  opacity: 0.9;
}
.back-btn {
  width: 50% !important ;
  height: auto;
  padding: 15px;
  border: none;
  background-color: #5d2977;
  color: white;
  cursor: pointer;
}
.back-btn:hover {
  opacity: 0.9;
}
.next-btn {
  width: 50% !important ;
  height: auto;
  padding: 15px;
  border: none;
  background-color: #5d2977;
  color: white;
  cursor: pointer;
}
.next-btn:hover {
  opacity: 0.9;
}
@media (max-width: 480px) {
  .assessment-container {
    padding: 10px;
  }

  .step-question,
  .step-user-details,
  .step-result {
    padding: 10px;
  }

  .submit-button {
    font-size: 14px;
  }
}
/*  */

/* Styles for smaller screens */

@media (max-width: 767px) {
  .main-page {
    /* padding: 1px; */
    /* min-height: 100% !important; */
    min-width: 100% !important;
    overflow: hidden !important;
    font-size: 14px;
    padding-top: 15%;
    padding-bottom: 15%;
  }
  .note{
    font-size: 7px !important;
    margin: 0 !important;
    font-weight: bold;
  }
  .top-result img{
  /* max-width: 250px; */
  height: 200px;
  width: 270px;
  padding: 0px !important;
  margin: 0 !important;
  
}
.top-result{
  padding: 1px;
}
  .assessment-container {
    width: 90%;
    height: 90%;
    padding: 10 10px;
  }
  .step-result-chart{
  display: flex;
  flex-direction: column;
  gap: 0;
}
.bottom-result-step{
  height: 200px;
  display: flex;
  flex-direction: column;
  gap: 0;
  width: 100%;
}
.bottom-result-step-text{
  width: 94% !important;
}
.bottom-result-step-img{
  display: none;
}
  /* Add other styles for smaller screens here */
}
.modal {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content-in {
  background-color: #f3f3f3;
  padding: 20px;
  width: 350px;
  border: none;
  border-radius: 5px;
}

</style>
