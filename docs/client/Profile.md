
# Profile 
## Overview

The `Profile` component provides an overview of the player's current status in the game, including their role, score, budget, round number, and a countdown timer. The information displayed is dynamically based on the player's role (either a producer or consumer) and the current stage of the game.

## Table of Contents

- [Component Structure](#component-structure)
- [Hooks](#hooks)
- [Conditional Rendering](#conditional-rendering)
- [Styling](#styling)

---

## Component Structure

The `Profile` component is made up of several sections:
1. **Round Info**: Displays the current round number.
2. **Budget**: Shows the player's budget (capital for producers or wallet for consumers).
3. **Role**: Displays the player's role (producer or consumer).
4. **Score**: Shows the player's current score.
5. **Timer**: Displays the remaining time for the current stage, unless the player is in the `ProducerChoice` stage.

```jsx
import React from "react";
import { usePlayer, useRound, useStage } from "@empirica/core/player/classic/react";
import { Avatar } from "./components/Avatar";
import { Timer } from "./components/Timer";
```

---

## Hooks

### `usePlayer`

```js
const player = usePlayer();
```

- **Description**: Retrieves the current player object, including details such as the player’s role, wallet (for consumers), capital (for producers), and score.
- **Returns**: An object representing the current player.

### `useRound`

```js
const round = useRound();
```

- **Description**: Retrieves information about the current round, such as the round name.
- **Returns**: An object representing the current round.

### `useStage`

```js
const stage = useStage();
```

- **Description**: Retrieves information about the current stage, such as the stage name.
- **Returns**: An object representing the current stage.

---

## Conditional Rendering

### Timer

The `Timer` component is displayed only if the player is not in the `ProducerChoice` stage. The timer indicates the remaining time for the current stage.

```jsx
{stage.get("name") !== "ProducerChoice" && (
  <>
    <p>Time</p>
    <span className="ml-1 p-1 px-2 bg-blue-600 rounded-full text-white">
      <Timer />
    </span>
  </>
)}
```

### Role and Budget

The budget is displayed as either the player's **capital** (for producers) or **wallet** (for consumers). The player's role is dynamically shown based on their game role.

```jsx
const money = player.get("role") === "consumer" ? player.get("wallet") : player.get("capital");
```

---

## Styling

The `Profile` component uses Tailwind CSS for styling, ensuring responsiveness and adaptability across different screen sizes. The component is wrapped in a flex container that arranges the elements horizontally with appropriate spacing.

### Layout

```html
<div
  id="Profile-container"
  className="
    flex justify-between self-center
    border-2 border-black bg-white rounded-full
    desktop:w-desktop tablet:w-tablet w-96
    min-h-12 desktop:min-h-top tablet:min-h-top
    text-mobile desktop:text-desktop tablet:text-tablet
  "
>
```

- **`flex`**: Ensures that the child elements are laid out in a row.
- **`border-2`**: Adds a solid 2px black border around the profile container.
- **`bg-white`**: Sets the background color to white.
- **`rounded-full`**: Gives the container rounded edges.
- **`desktop:w-desktop tablet:w-tablet w-96`**: Adjusts the width based on the screen size (desktop, tablet, or mobile).

### Individual Sections

Each section (round info, budget, role, score, and timer) is placed inside flex containers to align the content horizontally.

```html
<div id="profile-left" className="flex items-center justify-center ml-3">
  <p>Round</p>
  <span className="ml-1 p-1 px-2 bg-blue-600 rounded-full text-white">
    {round ? round.get("name").charAt(5) : ""}
  </span>
</div>
```

- **`items-center`**: Aligns the content vertically in the center.
- **`bg-blue-600 rounded-full text-white`**: Styles the round number, budget, role, and score indicators with a blue background and white text, wrapped in a rounded pill-shaped container.

---

## Conditional Display of Information

The component displays different information depending on the player's role and the current stage of the game:
- **Consumers**: See their wallet balance.
- **Producers**: See their capital.
- **Timer**: Only appears when the player is not in the `ProducerChoice` stage.

---
```