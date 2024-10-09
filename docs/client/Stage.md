
# Stage 
## Overview

The `Stage` component is responsible for determining and rendering the current stage of the game for the player. Depending on the stage name, it loads the appropriate component, such as `SelectRole`, `Tutorial`, `ConsumerChoice`, `ProducerChoice`, `Feedback`, or `Results`. It also manages waiting screens when a player has submitted their stage but is waiting for others to complete it.

## Table of Contents

- [Component Structure](#component-structure)
- [Hooks](#hooks)
- [Stage Loading](#stage-loading)
- [Switch Case for Stages](#switch-case-for-stages)
- [Conditional Rendering](#conditional-rendering)
- [Styling](#styling)

---

## Component Structure

The `Stage` component first checks if the player has already submitted their stage. If they have, it displays a waiting message. If the player hasn't submitted, it renders the appropriate stage of the game based on the stage name.

```jsx
import {
  usePlayer,
  usePlayers,
  useRound,
  useStage,
} from "@empirica/core/player/classic/react";
import { Loading } from "@empirica/core/player/react";
import React from "react";
import { SelectRole } from "./stages/SelectRole";
import { Tutorial } from "./stages/Tutorial";
import { ConsumerChoice } from "./stages/ConsumerChoice";
import { ProducerChoice } from "./stages/ProducerChoice";
import { Feedback } from "./stages/Feedback.jsx";
import { Results } from "./stages/Results.jsx";
import { Timer } from "./components/Timer.jsx";
```

---

## Hooks

### `usePlayer`, `usePlayers`, `useRound`, `useStage`

The following hooks are used to retrieve information about the current game state:

- **`usePlayer`**: Retrieves the current player's data, including their stage status.
- **`usePlayers`**: Retrieves the list of players, used to check how many players are in the game.
- **`useRound`**: Retrieves the current round data, though it is not explicitly used in this component.
- **`useStage`**: Retrieves the current stage data, such as the stage name and status.

---

## Stage Loading

### Handling Player Submissions

If the player has already submitted their stage, the component checks whether other players are still completing the stage. If the player is alone, it shows a loading screen, otherwise, it waits for other players to finish.

```jsx
if (player.stage.get("submit")) {
  if (players.length === 1) {
    return <Loading />;
  }
  return (
    <div className="card-combined">
      <div className="text-center text-gray-400">
        Please wait for other player(s).
      </div>
      {stage.get("name") !== "ProducerChoice" ? (
        <p>Time left until next stage:</p>
        <span className="bg-blue-600 rounded-full">
          <Timer />
        </span>
      ) : (
        <p>You must wait until the producer(s) are done making a product!</p>
      )}
    </div>
  );
}
```

---

## Switch Case for Stages

The `switch` statement determines which game stage to render based on the current stage name.

```jsx
switch (stage.get("name")) {
  case "SelectRole":
    return <SelectRole />;
  case "Tutorial":
    return <Tutorial />;
  case "ConsumerChoice":
    return <ConsumerChoice />;
  case "ProducerChoice":
    return <ProducerChoice />;
  case "Feedback":
    return <Feedback />;
  case "Results":
    return <Results />;
  default:
    return <Loading />;
}
```

- **`SelectRole`**: Renders the `SelectRole` stage where players choose their roles.
- **`Tutorial`**: Renders the tutorial for the players.
- **`ConsumerChoice`**: Renders the consumer's choice stage.
- **`ProducerChoice`**: Renders the producer's choice stage.
- **`Feedback`**: Renders the feedback stage where players review and give feedback.
- **`Results`**: Renders the results of the game.

---

## Conditional Rendering

### Handling Different Stages

The component handles conditional rendering of content based on the current stage. For example, if the stage is not `ProducerChoice`, the timer is shown, while in the `ProducerChoice` stage, players must wait for the producers to finish making their product.

```jsx
{stage.get("name") !== "ProducerChoice" ? (
  <p>Time left until next stage:</p>
  <span className="bg-blue-600 rounded-full">
    <Timer />
  </span>
) : (
  <p>You must wait until the producer(s) are done making a product!</p>
)}
```

---

## Styling

The component uses Tailwind CSS for styling and layout management. The layout ensures proper positioning of the content and centers elements based on the stage.

```html
<div className="card-combined flex flex-col justify-center text-center relative h-[20rem] rounded-full my-6 p-8">
```

- **`flex flex-col`**: Ensures that the content is arranged vertically.
- **`text-center`**: Centers the text horizontally.
- **`h-[20rem]`**: Sets a fixed height of 20rem for the card.
- **`rounded-full`**: Gives the card rounded edges.
- **`p-8`**: Adds padding around the content.

---
```