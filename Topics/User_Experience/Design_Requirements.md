# **Design Requirements**

# Table of Contents

### [Introduction](#introduction)

### [Motivation](#motivation)

### [Functional Design Requirements](#functional-design-requirements)

### [Non-Functional Design Requirements](#non-functional-design-requirements)

### [Extracting Action Items](#extracting-action-items)

### [References](#references)

### [Additional Resources](#additional-resources)

## **Introduction**

Design requirements are expressions which outline **“what”** the desired product should achieve and **“how”** it should achieve them, corresponding to the usefulness and usability of designs.

The usefulness of a product is addressed by the question “Are we designing the right thing?” [1], which considers whether this product is needed by users. 
- This aspect focuses on ensuring that developers are creating solutions for actual pain points that users have, rather than creating solutions based on assumptions that they make about the user experience.

The usability of a product is addressed by the question “Are we designing the thing right?” [1], which considers whether users are able to use the product.
- This aspect focuses on creating products that is the most satisfactory in terms of the goals of end users.

To achieve this goal, design requirements include specifications related to user interfaces, navigation, visual design, and additional elements which contribute to the satisfaction of end users and overall usability. This makes design requirements an integral part of the user experience.

## **Motivation**

When working with partner clients, scoping the problem that clients want to address and understanding the requirements that their ideal solution entails is very important. Establishing design requirements is highly valuable for CSC301 students, who need to create minimum-viable-products (MVPs) from scratch.

### **1. Client Needs** 

After synthesizing user stories, extracting the functional and non-functional design requirements for any design (whether it is a web-application or mobile application) will allow students to gauge the specific features that they will be working on in order to create an MVP that accurately addresses client needs.

Clients taking part in the process of extracting design requirements with developers can be beneficial for both parties, due to several reasons:

- **Clear Understanding of Business Goals**: Clients have a deep understanding of their business goals and objectives. Their involvement ensures that the design requirements align closely with this overall vision.
- **User-Centric Design**: Clients have insights into the preferences and expectations of end users, especially if they have been in the same industry for a long time. By participating in the design requirements extraction process, clients can contribute valuable input to ensure that the final product meets user needs and expectations.
- **Feedback Loop**: Involving clients early in the design process help establishes an iterative approach. This continuous feedback loop allows for adjustments and refinements throughout the development phases.
- **Empowerment and Engagement**: Clients that actively participate in the design process feel more empowered and engaged, which can lead to a better sense of partnership between the client and the development team.

### **2. Action Items**

The design requirements make sure that regardless of the platform the solution is being presented in, the main problem the clients present is solved and the user needs are met. They can also be dissected to identify how the requirements match to various front-end, back-end, or database tasks, allowing for students to scope actionable task items.

### **3. Communication**

Accurately defining and documenting requirements makes sure that the communication between team members and clients are not lost and can be referred to later in the project to see if they have been met, especially after usability studies. Benefits include;

- **Clearer communication**: Defining requirements ensures that the developer team and the client are on the same page and they can identify misunderstandings early on.
- **Asynchronous communication**: With shared understanding and written documentation, certain questions can be addressed asynchronously, to see if a certain feature matches the clients design requirements.
- **Efficiency**: There is less time spent on actively thinking about what the client had wanted. The written documentation can be easily referred to in later processes and can be used to hold clients accountable as well.

## **Functional Design Requirements**

Functional design requirements specify the goals of the desired product. They focus on the “usefulness” of the product.

Examples include;
- The system must enable users to verify their accounts using their phone number.
- The system should support backward and forward navigation in the research article to follow the natural order of sections.
- The system must allow visitors to sign up for the newsletter by inputting their email. 

### **Relation to User Stories**

Functional requirements are not the same as user stories. In fact, user stories can be used to derive design requirements while centering user needs. An [example](https://www.nuclino.com/articles/functional-requirements) of this is as follows: 

“User story: As an existing user, I want to be able to log into my account.

Functional requirements:
- The system must allow users to log into their account by entering their email and password.
- The system must allow users to log in with their Google accounts.
- The system must allow users to reset their password by clicking on "I forgot my password" and receiving a link to their verified email address.” [2]

## **Non-Functional Design Requirements**

Non-functional design requirements specify the methods used to achieve the goals of the desired product. They focus on the “usability” of the product and provide constraints on the system and its development. 

Examples include;
- The system should ensure security and privacy in the account verification process, adhering to industry standards for data protection and encryption.
- The system should display as much text as possible, without compromising accessibility standards.
- When the user presses the sign-up button, the system should load the confirmation screen in two seconds.

As seen in the examples, non-functional design requirements impose constraints on how the functional requirements are implemented in terms of *performance, security, reliability, scalability, and portability.*

## **Extracting Action Items**

Design requirements make it efficient for developers to extract specific front-end, database, and back-end tasks, which is useful for CSC301 students who might have sub-teams dedicated to these components. For example, from the functional requirement "The system must enable users to verify their accounts using their phone number" and the related non-functional requirement "The system should ensure security and privacy in the account verification process, adhering to industry standards for data protection and encryption", developers infer that they need to complete the following tasks:

#### 1. Front-End Tasks
- Design and implement an interface for account verification
- Create a component for users to input their phone number
- Implement client-side validation for phone number format
- Display appropriate confirmation/error messages to users based on verification
- Integrate any libraries or frameworks to handle user input and interactions

#### 2. Back-End Tasks
- Implement server-side logic to handle the account verification process
- Receive and validate the phone number input from the user
- Generate a verification code and associate it with the user account
- Send the verification code to the phone number of the user through an SMS service
- Implement logic to receive and verify the code entered by the user

#### 3. Database Tasks
- Design a database schema to store user account information, including phone numbers and verification status
- Implement database queries to insert, update, and retrieve user information during the verification process
- Ensure data integrity and security measures, such as encryption for sensitive information.

## Conclusion

Design requirements are helpful not only at the beginning of the design process, but also throughout the entire development cycle.  When determining which features to implement or prioritize at each reiteration, assessing these decisions against the design requirements ensures centering client needs and main functionalities the desired system is trying to achieve. During low-fidelity prototyping, high-fidelity prototyping (such as MVPs), development, and following usability studies, the team can assess whether the design requirements were met and what changes can be made at each subsequent step to better address them.

## References

[1] Fanny Chevalier. 2023. Lecture 05.4: Synthesizing Actionable Insights - Design Requirements. CSC318. (November 2023).

[2] Nuclino. A Guide to Functional Requirements (with Examples). Retrieved November 27, 2023 from https://www.nuclino.com/articles/functional-requirements 

## Additional Resources

[Functional Requirements Examples and Templates](https://www.jamasoftware.com/requirements-management-guide/writing-requirements/functional-requirements-examples-and-templates#:~:text=A%20functional%20requirement%20is%20a,details%20of%20the%20product's%20features)

[Functional vs. Nonfunctional Requirements](https://www.jamasoftware.com/requirements-management-guide/writing-requirements/functional-vs-non-functional-requirements)

[What are Non-Functional Requirements?](https://www.jamasoftware.com/requirements-management-guide/writing-requirements/how-non-functional-requirements-impact-product-development)

