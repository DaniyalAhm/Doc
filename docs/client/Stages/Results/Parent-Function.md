```markdown
# Results Component Documentation

## Overview

The `Results` component serves as the parent component that conditionally renders either the `ProducerResults` or `ConsumerResults` components based on the player's role in the game. It fetches necessary data using hooks and decides which child component to display depending on whether the player is a producer or a consumer.

## Table of Contents

- [Component Structure](#component-structure)
- [Functions](#functions)
  - [handleSubmit](#handlesubmit)
- [Hooks](#hooks)
- [Styling](#styling)

---

## Component Structure

The `Results` component checks the player's role and renders the appropriate results view (either producer or consumer). If the player is a consumer, it displays the `ConsumerResults`. If the player is a producer, it displays the `ProducerResults`.

```jsx
import React from "react";
import { usePlayer, usePlayers, useGame } from "@empirica/core/player/classic/react";
import { ProducerResults } from "./ProducerResults.jsx";
import { ConsumerResults } from "./ConsumerResults.jsx";

export function Results() {
  const player = usePlayer();
  const game = useGame();

  return (
    <div>
      {player.get("role") === "consumer" ? <ConsumerResults /> : <ProducerResults />}
    </div>
  );
}
```

- The `usePlayer` hook retrieves the current player's data, which includes their role (`producer` or `consumer`).
- Depending on the value of `player.get("role")`, either `ConsumerResults` or `ProducerResults` is rendered.

---

## Functions

### `handleSubmit`

This function is passed down to both the `ProducerResults` and `ConsumerResults` components to handle the submission of results at the end of the round. It resets the player's stock purchase status and marks the stage as submitted.

```js
const handleSubmit = () => {
  player.set("boughtStock", false);
  player.stage.set("submit", true);
};
```

- **Description**: Submits the player's results and proceeds to the next game stage.
- **Returns**: `void`

---

## Hooks

- **`usePlayer`**: Fetches the current player's data, including their role in the game.
- **`useGame`**: Fetches the game data, which can be used for game-wide treatments, round information, or specific configurations.
- **`usePlayers`** (optional): May be used to fetch all players, but in this parent component, only `usePlayer` is essential.

---

## Styling

This component itself doesn't have much styling but passes the logic to conditionally render `ProducerResults` or `ConsumerResults`. Both child components use Tailwind CSS for styling and handle their own layout.

```jsx
<div>
  {player.get("role") === "consumer" ? <ConsumerResults /> : <ProducerResults />}
</div>
```

- **Dynamic Rendering**: The role check (`player.get("role") === "consumer"`) dynamically decides which component to display.
```