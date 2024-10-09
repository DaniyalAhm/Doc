# CallBacks

This documentation covers the implementation of the `Empirica` game using `ClassicListenersCollector`. It describes how the game stages, rounds, and player roles are initialized and manipulated, as well as how certain events like the start or end of a round or stage are handled.

## Table of Contents

- [shuffleArray Function](#shufflearray-function)
- [getRandomName Function](#getrandomname-function)
- [onGameStart Listener](#ongamestart-listener)
- [onRoundStart Listener](#onroundstart-listener)
- [onStageStart Listener](#onstagestart-listener)
- [onStageEnded Listener](#onstageended-listener)
- [onRoundEnded Listener](#onroundended-listener)
- [onGameEnded Listener](#ongameended-listener)

---

## shuffleArray Function

This function shuffles an array of players in order to randomly assign roles (e.g., producer or consumer) to each player.

```javascript
function shuffleArray(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}
```

### Parameters

- `array`: The array of players to shuffle.

### Returns

- A shuffled array.

---

## getRandomName Function

This function returns a random brand name from the predefined `NAMES` array, which is used to assign random brand names to players.

```javascript
function getRandomName() {
  return NAMES[Math.floor(Math.random() * NAMES.length)];
}
```

### Returns

- A randomly selected name from the `NAMES` array.

---

## onGameStart Listener

This listener is triggered when the game starts. It initializes the game rounds and stages and assigns roles, attributes, and random names to players.

```javascript
Empirica.onGameStart(({ game }) => {
  const { playerCount, warrantTreatment, producerCount, consumerCount } = game.get("treatment");

  for (let i = 1; i <= 5; i++) {
    const round = game.addRound({ name: `Round${i}` });

    if (i === 1) {
      round.addStage({ name: "SelectRole", duration: 90 });
      round.addStage({ name: "Tutorial", duration: 120 });
    }

    round.addStage({ name: "ProducerChoice", duration: 10000 });
    round.addStage({ name: "ConsumerChoice", duration: 90 });
    round.addStage({ name: "Feedback", duration: 90 });
    round.addStage({ name: "Results", duration: 90 });
  }

  const shuffledPlayers = shuffleArray([...game.players]);

  shuffledPlayers.forEach((player, index) => {
    if (index < producerCount) {
      // Assign attributes for producers
      player.set("role", "producer");
      player.set("capital", initialCapital);
      player.set("reviews", [0, 0]);
      player.set("score", []);
      player.set("totalScore", [0]);
      player.set("profits", [0]);
      player.set("totalProfits", 0);
      player.set("currentQuality", "");
      player.set("productQuality", []);
      player.set("claims", [0, 0]);
      player.set("sold", []);
      player.set("changedBrand", []);
      player.set("brandName", getRandomName());
      player.set("warrants", []);
      player.set("challenges", []);
      player.set("challengesLost", []);
    } else {
      // Assign attributes for consumers
      player.set("role", "consumer");
      player.set("wallet", initialWallet);
      player.set("score", []);
      player.set("totalScore", [0]);
      player.set("roundScore", []);
      player.set("currentChallenge", []);
      player.set("challenged", []);
      player.set("claimsWon", []);
      player.set("cart", []);
      player.set("purchasedFrom", []);
      player.set("consumerReviews", {});
    }
  });
});
```

### Key Functions

- **`shuffleArray([...game.players])`**: Shuffles players and assigns roles.
- **`player.set()`**: Initializes attributes for both producers and consumers.

---

## onRoundStart Listener

Triggered when a round starts. It handles any round-specific logic, like resetting or updating attributes for producers and consumers.

```javascript
Empirica.onRoundStart(({ round }) => {
  const players = round.currentGame.players;
  for (const player of players) {
    if (stage.get("name") === "ProducerChoice") {
      if (player.get("changedBrand").slice(-1)[0] === true) {
        player.set("brandName", getRandomName());
        player.set("reviews", [0, 0]);
      }
    }
  }
});
```

---

## onStageStart Listener

Triggered at the start of a stage, this listener can be used to handle specific actions needed at the beginning of a stage.

```javascript
Empirica.onStageStart(({ stage }) => {
  // Implement logic to execute at the start of a stage
});
```

---

## onStageEnded Listener

Handles the end of a stage and performs cleanup or updates needed before moving to the next stage. It calculates profits and scores for producers and consumers.

```javascript
Empirica.onStageEnded(({ stage }) => {
  const players = stage.currentGame.players;

  for (const player of players) {
    if (stage.get("name") === "ProducerChoice") {
      if (player.get("role") === "producer") {
        let soldOnRound = [];
        for (const cons of players.filter(p => p.get("role") === "consumer")) {
          const purchasedFrom = cons.get("cart");
          if (purchasedFrom.indexOf(player.get("name")) !== -1) {
            soldOnRound.push(cons.get("name"));
          }
        }
        player.set("sold", [...player.get("sold"), soldOnRound]);
      }
    }

    if (stage.get("name") === "Feedback") {
      const producers = players.filter(p => p.get("role") === "producer");
      for (const prod of producers) {
        // Handle feedback processing logic
      }
    }
  }
});
```

### Key Functions

- **`player.set("sold")`**: Updates the producer's list of sold items for the current round.
- **`stage.get("name")`**: Switches logic based on the stage name, such as "ProducerChoice" or "Feedback".

---

## onRoundEnded Listener

Triggered when a round ends. This listener can handle any round-level cleanup, such as resetting variables or preparing for the next round.

```javascript
Empirica.onRoundEnded(({ round }) => {
  // Implement logic to execute when a round ends
});
```

---

## onGameEnded Listener

Triggered when the game ends. This listener is responsible for final actions like showing game results or saving game data.

```javascript
Empirica.onGameEnded(({ game }) => {
  // Implement logic to execute when the game ends
});
```
```