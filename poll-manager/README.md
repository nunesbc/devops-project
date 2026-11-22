# Poll manager

## **Dependencies**

* npm: v 8.14.0
* node v 16.14.2
* cors: "^2.8.5",
* dotenv: "^18.0.10",
* express: "^4.18.1",
* mysql2: "^3.24.6",
* sequelize: "^6.21.3"

----

## **Project Installation**

### Steps to start the back-end

1. After cloning this repository, navigate to the `server` directory (`cd server`) and run `npm install` to install the dependencies.

2. Update the database configuration settings with your credentials under the `development` key located in the `config/config.json` file.

3. Rub `npx sequelize-cli db:create` to create the database

4. Run `npx sequelize-cli db:migrate` to create the `Polls` and `Options` tables in the database.

5. Run `node server` to start the back-end.

### Steps to start the front-end
1. After cloning this repository, navigate to the `client` directory (`cd client`) and run `npm install` to install the dependencies.
2. Run `npm start` to start the front-end.

---

## Back-end Project
The back-end project is located in the `server` folder and was developed using Node.js. The **Sequelize ORM** was used to manage the database.

### `models` folder
In the `models` folder, you will find `options.js` and `polls.js`. These files represent the database tables created for this project.

In the `Polls` class, there is a `getPollStatus()` method that verifies whether the current time falls within the active period of the poll. Whenever instantiated, this method returns an object containing the status of the poll (string) and whether it is active (boolean). The object returned by this method is saved in the **`status`** variable of the poll.

> *The **`status`** variable is a dynamic virtual variable that exists only in `polls.js`. No actual database column exists in the `polls` table for this variable.*

### `crud.js`
The methods responsible for **displaying, creating, deleting, and updating** table data are located in the `crud` file.

The `updateOptionScore()` method counts the votes received by each option in a poll. This function increments the option score whenever invoked using Sequelize's `increment()` method. You can read more about the increment function on [Sequelize Documentation](https://sequelize.org/docs/v6/core-concepts/model-instances/).

> *Although the `deletePoll()`, `updatePoll()`, and `deleteOption()` methods are fully functional inside `crud.js`, they are not used in the front-end. Therefore, no API endpoints were created for them.*

### `server.js`
`server.js` contains all API endpoints connecting the front-end interface with the backend functions in `crud.js`.

---

## Front-end Project
The front-end project is located in the `client` folder. It was developed using **React.js** and **Material UI**, as these libraries facilitate building responsive applications ideal for mobile devices.

### `src/components` folder
* **`NavBar.js`**: The navigation bar component.
* **`Operations.js`**: File containing utility functions used across various project components.
* **`PollsPainel.js`**: Manages what is displayed to the user. The `PollsPainel` component shows the form for creating a new poll, a specific poll, or all polls based on pagination.
* **`/polls`**: Directory containing the components that structure poll displays.
* **`/createPoll`**: Directory containing the components that structure the poll creation form.

### `src/components/polls` folder
* **`AllPolls.js`**: Displays titles of all existing polls in the database and allows users to select which poll to view.

### `src/components/polls/components/Poll.js`
Displays details of a specific poll and utilizes the `Option.js` component.

### `src/components/polls/components/Option.js`
Represents a single option within a poll. A `.map()` iteration is run inside `Poll` to render all options for that poll. Options are displayed as checkboxes, accessible for user interaction only if the poll is active and the user has not voted yet.

The user's vote is saved in `localStorage` whenever they vote on a poll. The vote is stored as an object containing the ID of the selected option and the poll ID to which it belongs.

> *The entire project was designed to simulate a voting system where each user can vote only once per poll. Votes are saved in `localStorage` to eliminate the need for storing registered user accounts in the database.*

* **Note 1**: To convert the application into an open voting system where users can vote unlimited times, remove or comment out the vote-saving logic in `localStorage`, clear existing storage with `localStorage.clear()`, and disable the vote-check logic.
* **Note 2**: Users can vote multiple times if using incognito/private browser sessions because data generated in private windows is not permanently stored in `localStorage`.

Votes are verified using the `checkVote()` method from `Operations.js`. The output is used across several components:
* **`AllPolls.js`**: Informs the user if they have already voted on a poll.
* **`Poll.js`**: Checks if a user has voted on a specific poll and disables voting controls if true.
* **`Option.js`**: Identifies which option was voted for and keeps it selected.

> *This component also includes a `setInterval()` timer that calls `getOptionsByPollId()` to retrieve options every minute. This allows fetching score data and showing live total vote counts in real time.*

### `src/components/createPoll/CreatePoll.js`
The form component for creating new polls, utilizing `PollDate.js` and `SavePoll.js`.

`CreatePoll.js` gathers all required information for a poll and passes it to the `SavePoll` component.

The `SavePoll` component contains a button invoking `createPoll()` and `createOption()` from `crud.js`. These functions save the new poll and options to the database.

The submission button in `SavePoll` is enabled only when no fields in `CreatePoll.js` are left empty. Validation is handled by `checkNewPollData()` in `Operations.js`, ensuring no empty rows are created in the database tables.
