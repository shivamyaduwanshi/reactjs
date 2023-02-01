import React, { useState } from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";
import * as Yup from "yup";

const StepCounter = ({ currentStep, stepsLength }) => (
  <p>
    Step {currentStep} of {stepsLength}
  </p>
);

const BasicInfo = ({ nextStep, isValid,dirty }) => (
  <>
    <h3>Basic Info</h3>
    <Field name="name" placeholder="Name" />
    <ErrorMessage name="name" />
    <Field name="email" type="email" placeholder="Email" />
    <ErrorMessage name="email" />
    <Field name="mobile" placeholder="Mobile" />
    <ErrorMessage name="mobile" />
    <button type="button" disabled={!isValid || !dirty} onClick={nextStep}>
      Next
    </button>
  </>
);

const EducationDetails = ({ nextStep, prevStep , isValid,dirty }) => (
  <>
    <h3>Education Details</h3>
    <Field name="highestDegree" placeholder="Highest Degree" />
    <ErrorMessage name="highestDegree" />
    <Field name="passingYear" placeholder="Passing Year" />
    <ErrorMessage name="passingYear" />
    <Field name="universityName" placeholder="University Name" />
    <ErrorMessage name="universityName" />
    <button type="button" onClick={prevStep}>
      Prev
    </button>
    <button type="button" disabled={!isValid || !dirty}  onClick={nextStep}>
      Next
    </button>
  </>
);

const BankAccountDetails = ({ prevStep, handleSubmit }) => (
  <>
    <h3>Bank Account Details</h3>
    <Field name="bankName" placeholder="Bank Name" />
    <ErrorMessage name="bankName" />
    <Field name="accountNumber" placeholder="Account Number" />
    <ErrorMessage name="accountNumber" />
    <Field name="bankBranch" placeholder="Bank Branch" />
    <ErrorMessage name="bankBranch" />
    <button type="button" onClick={prevStep}>
      Prev
    </button>
    <button type="submit">Submit</button>
  </>
);

const MultiStepForm = () => {
  const [step, setStep] = useState(1);
  const steps = [
    {
      id: 1,
      component: BasicInfo,
      validationSchema: Yup.object().shape({
        name: Yup.string().required("Name is required"),
        email: Yup.string()
          .email("Invalid email")
          .required("Email is required"),
        mobile: Yup.string().required("Mobile is required")
      })
    },
    {
      id: 2,
      component: EducationDetails,
      validationSchema: Yup.object().shape({
        highestDegree: Yup.string().required("Highest Degree is required"),
        passingYear: Yup.string().required("Passing Year is required"),
        universityName: Yup.string().required("University Name is required")
      })
    },
    {
      id: 3,
      component: BankAccountDetails,
      validationSchema: Yup.object().shape({
        bankName: Yup.string().required("Bank Name is required"),
        accountNumber: Yup.string().required("Account Number is required"),
        bankBranch: Yup.string().required("Bank Branch is required")
      })
    }
  ];

  const nextStep = () => setStep(step + 1);
  const prevStep = () => setStep(step - 1);
  const currentStep = steps.find(s => s.id === step);

  return (
    <Formik
      initialValues={{}}
      validationSchema={currentStep.validationSchema}
      onSubmit={values => {
        console.log(values);
      }}
    >
      {({ isValid, dirty }) => (
        <Form>
          <StepCounter currentStep={step} stepsLength={steps.length} />
          {React.createElement(currentStep.component, {
            nextStep,
            prevStep,
            handleSubmit: () => {
              if (isValid && dirty) {
                nextStep();
              }
            },
            isValid,
            dirty
          })}
        </Form>
      )}
    </Formik>
  );
};

export default MultiStepForm;
