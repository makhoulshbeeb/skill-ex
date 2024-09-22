<img src="./readme/title1.svg"/>

<br><br>

<!-- project philosophy -->
<img src="./readme/title2.svg"/>

> SkillEx is an innovative platform that connects people with different skills and talents, allowing them to exchange services without the need for money. Users can offer their expertise in various fields, such as graphic design, language tutoring, coding, or practically anything, and in return, receive help in areas where they need assistance. The app fosters a community of mutual support and learning, making skill acquisition and service access more affordable and collaborative.

### User Stories
- As a user, I want to learn new skills without having to hire expensive tutors.
- As a user, I want to see user ratings and reviews, so I can ensure I'm exchanging services with reliable and skilled individuals.
- As a user, I want to set up virtual meetings and workshops, so I can learn and collaborate with others in real-time.

- As an admin, I want to be able to monitor and delete inappropriate users and reviews.
- As an admin, I want to be able to add, update and delete skill categories.

<br><br>
<!-- Tech stack -->
<img src="./readme/title3.svg"/>

###  SkillEx is built using the following technologies:

This project is built using the MERN stack ([MongoDB](https://www.mongodb.com/), [Express](https://expressjs.com/), [React](https://react.dev/), [Node.js](https://nodejs.org/en)) and integrates real-time features with [Socket.io](https://socket.io/) and [WebRTC](https://webrtc.org/) for video conferencing.

- MERN Stack: This website is developed using the MERN technology stack, which ensures a seamless development process by using JavaScript for both the front-end and back-end. React powers the dynamic user interface, Node.js and Express handle the server-side logic, and MongoDB provides efficient and flexible database management.

- Real-time Communication: The project utilizes Socket.io for real-time, bidirectional communication between clients and the server. This enables instant messaging, live notifications, and seamless updates across all connected users.

- In-App Video Conferencing: The website implements WebRTC for real-time peer-to-peer video conferencing. This allows users to initiate and join video calls directly within the platform, providing high-quality video and audio without the need for external plugins or applications.

- Persistent Storage: For data persistence, the project uses MongoDB as the NoSQL database. MongoDB allows for flexible schema design, making it ideal for dynamic and evolving data requirements, such as storing user profiles, reviews and chat history.

<br><br>
<!-- UI UX -->
<img src="./readme/title4.svg"/>


> We designed SkillEx using wireframes and mockups, iterating on the design until we reached the ideal layout for easy navigation and a seamless user experience.

- Project Figma design [figma](https://www.figma.com/design/KEV6m8171V7IqSmB67XPfS/SkillEx?node-id=1-4&t=7Hl4STyg6LmidYRF-1)


### Mockups
|   |   |   |
| ---| ---| ---|
| Login Screen  | Sign up Screen |  Home screen  |
| ![Landing](./readme/demo/1440x1024.png) | ![fsdaf](./readme/demo/1440x1024.png) | ![fsdaf](./readme/demo/1440x1024.png) |
| User screen  | Chat Screen | Video Screen |
| ![Landing](./readme/demo/1440x1024.png) | ![fsdaf](./readme/demo/1440x1024.png) | ![fsdaf](./readme/demo/1440x1024.png) |
| Admin Categories screen  | Admin Users Screen | Admin Users Screen 2 |
| ![Landing](./readme/demo/1440x1024.png) | ![fsdaf](./readme/demo/1440x1024.png) | ![fsdaf](./readme/demo/1440x1024.png) |

<br><br>

<!-- Database Design -->
<img src="./readme/title5.svg"/>

###  Architecting Data Excellence: Innovative Database Design Strategies:

#### Users Schema:

```js
const userSchema = new mongoose.Schema(
    {
        displayName: {
            type: String,
            required: true,
        },
        username: {
            type: String,
            required: true,
            unique: true,
            validate: {
                validator: (username) => { return /^[a-zA-Z0-9_]+$/.test(username) },
                message: "Please provide a valid username",
            },
            minlength: 4,
            maxlength: 24,
        },
        email: {
            type: String,
            required: true,
            unique: true,
            lowercase: true,
            validate: {
                validator: (email) => validator.isEmail(email),
                message: "Please provide a valid email",
            },
        },
        password: {
            type: String,
            required: true,
            minlength: 8,
        },
        gender: {
            type: String,
            required: true,
            enum: ["male", "female", "other"],
        },
        picture: {
            type: String,
            default: "",
        },
        bio: {
            type: String,
            default: "",
        },
        avgRating: {
            type: Number,
            min: 0,
            max: 5,
            default: 0
        },
        reviews: {
            type: [
                {
                    type: mongoose.Schema.Types.ObjectId,
                    ref: "Review",
                },
            ],
            default: [],
        },
        teach: {
            type: [
                {
                    category: {
                        type: mongoose.Schema.Types.ObjectId,
                        ref: "Category"
                    },
                    endorsements: [{
                        type: mongoose.Schema.Types.ObjectId,
                        ref: "User"
                    }],
                    _id: false,
                },

            ],
            default: [],
        },
        learn: {
            type: [
                {
                    category: {
                        type: mongoose.Schema.Types.ObjectId,
                        ref: "Category"
                    },
                    _id: false,
                }
            ],
            default: [],
        },
        role: {
            type: String,
            enum: ['User', 'Admin'],
            default: 'User'
        }
    },
    { timestamps: true }
);
```

#### Reviews Schema:

```js
const reviewSchema = new mongoose.Schema(
    {
        reviewerId: {
            type: mongoose.Schema.Types.ObjectId,
            ref: "User",
            required: true,
        },
        receiverId: {
            type: mongoose.Schema.Types.ObjectId,
            ref: "User",
            required: true,
        },
        feedback: {
            type: String,
            required: true,
        },
        rating: {
            type: Number,
            required: true,
            min: 1,
            max: 5,
        }
    },
    { timestamps: true }
);
reviewSchema.index({ reviewerId: 1, receiverId: 1 }, { unique: true })
```

#### Categories Schema:

```js
const categorySchema = new mongoose.Schema(
    {
        name: {
            type: String,
            required: true,
            unique: true,
        },
        picture: {
            type: String,
            default: "https://e7.pngegg.com/pngimages/75/866/png-clipart-category-management-organization-retail-management-miscellaneous-text-thumbnail.png",
        },
    },
    { timestamps: true }
);
```

#### Chats Schema

```js
const chatSchema = new mongoose.Schema(
    {
        participants: {
            type: [
                {
                    type: mongoose.Schema.Types.ObjectId,
                    ref: "User",
                },
            ],
        },
        messages: [
            {
                type: mongoose.Schema.Types.ObjectId,
                ref: "Message",
                default: [],
            },
        ],
    },
    { timestamps: true }
);
```

#### Messages Schema

```js
const messageSchema = new mongoose.Schema(
    {
        senderId: {
            type: mongoose.Schema.Types.ObjectId,
            ref: "User",
            required: true,
        },
        receiverId: {
            type: mongoose.Schema.Types.ObjectId,
            ref: "User",
            required: true,
        },
        message: {
            type: String,
            required: true,
        },
    },
    { timestamps: true }
);
```

<br><br>


<!-- Implementation -->
<img src="./readme/title6.svg"/>


### User Screens (Web)
|   |   |   |
| ---| ---| ---|
| Login Screen  | Sign up Screen |  Home screen  |
| ![Login](./readme/demo/LoginPage.gif) | ![Sign up](./readme/demo/SignupPage.gif) | ![Home](./readme/demo/HomePage.gif) |
| User screen  | Chat Screen | Video Screen |
| ![User](./readme/demo/UserPage.gif) | ![Chat](./readme/demo/ChatPage.gif) | ![Video](./readme/demo/VideoPage.gif) |
| Profile Screen |
| ![Profile](./readme/demo/ProfilePage.gif) |

### Admin Screens (Web)
| Admin Categories screen  | Admin Users Screen |
| ---| ---|
| ![Admin Categories](./readme/demo/AdminCategoriesPage.gif) | ![Admin Users](./readme/demo/AdminUsersPage.gif) |

<br><br>


<!-- Prompt Engineering -->
<!-- <img src="./readme/title7.svg"/>

###  Mastering AI Interaction: Unveiling the Power of Prompt Engineering:

- This project uses advanced prompt engineering techniques to optimize the interaction with natural language processing models. By skillfully crafting input instructions, we tailor the behavior of the models to achieve precise and efficient language understanding and generation for various tasks and preferences.

<br><br> -->

<!-- AWS Deployment -->
<img src="./readme/title8.svg"/>

###  Efficient Deployment: Unleashing the Potential with AWS Integration:

- This project leverages AWS EC2 deployment strategies to seamlessly integrate and deploy natural request processing and handling. With a focus on scalability, reliability, and performance, we ensure that the flow of information is dynamic and proactive to reflect a seemless user experience.

<br><br>

<!-- Unit Testing -->
<img src="./readme/title9.svg"/>

###  Precision in Development: Harnessing the Power of Unit Testing:

- This project employs rigorous unit testing methodologies to ensure the reliability and accuracy of code components. By systematically evaluating individual units of the software, we guarantee a robust foundation, identifying and addressing potential issues early in the development process.

- In order to ensure the effeciency and reliability of all backend processes, api calls have been mapped into a [Postman project](https://skill-ex.postman.co/workspace/skill-ex-Workspace~3b83ee7d-9566-4af3-8f43-268a4f049d16/collection/36926328-d6f09023-f6a0-4bf3-930d-0cf640b9abaa?action=share&creator=36926328) where they underwent rigorous and frequent testing.

<br><br>


<!-- How to run -->
<img src="./readme/title10.svg"/>

> To set up SkillEx locally, follow these steps:

### Installation

1. Clone the repo
    ```sh
   git clone https://github.com/makhoulshbeeb/skill-ex
   ```
2. Install NPM packages and run backend
   ```sh
   cd skill-ex-back
   npm install
   npm run dev
   ```
   
3. Follow the .env.example files on each submodules to set up your environment variables

4. Install NPM packages and run frontend
   ```sh
   cd skill-ex-front/SkillEx
   npm install
   npm run dev
   ```

Now, you should be able to run SkillEx locally and explore its features.
