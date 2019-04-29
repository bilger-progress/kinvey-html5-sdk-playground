<template>
  <div class="hello">
    <h6>{{ msg }}</h6>
  </div>
</template>

<script>
/* eslint-disable */

const Kinvey = require("kinvey-html5-sdk");
const intervalPromise = require("interval-promise");

const APP_KEY = "xxx";
const APP_SECRET = "xxx";
const USER_NAME = "xxx";
const USER_PASSWORD = "xxx";
const COLLECTION_NAME = "xxx";
const ENTITY_ID = "xxx";
const TOTAL_ITERATIONS = 0;
const TIMEFRAME_ITERATIONS = 0;

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  mounted: () => {
    // Set-up backend.
    Kinvey.init({
      appKey: APP_KEY,
      appSecret: APP_SECRET
    });
    // Start the process.
    Kinvey.ping()
      .then(response => {
        console.log(
          "Kinvey Ping Success. Kinvey Service is alive, version: " +
            response.version +
            ", response: " +
            response.kinvey
        );
        if (Kinvey.User.getActiveUser() === null) {
          console.log("Logging-in the user.");
          return Kinvey.User.login(USER_NAME, USER_PASSWORD);
        }
        console.log("User already logged-in.");
        return Kinvey.User.getActiveUser();
      })
      .then(user => {
        console.log(`User: ${user.data.username}`);
        const dataStore = Kinvey.DataStore.collection(
          COLLECTION_NAME,
          Kinvey.DataStoreType.NETWORK
        );
        console.log("Starting DataStore operations.");
        console.time("session");
        intervalPromise(
          async (iteration, stop) => {
            try {
              const operationResult = await dataStore.save({
                _id: ENTITY_ID,
                test_field: iteration
              });
              await console.log(operationResult);
              if (iteration === TOTAL_ITERATIONS) {
                console.timeEnd("session");
              }
            } catch (exc) {
              console.error("Error occured. Please check logs below.");
              console.error(exc);
              console.error("Stopping DataStore operations.");
              stop();
              console.timeEnd("session");
            }
          },
          TIMEFRAME_ITERATIONS,
          { iterations: TOTAL_ITERATIONS }
        );
      })
      .catch(error => {
        console.error(error);
      });
  }
};
</script>
