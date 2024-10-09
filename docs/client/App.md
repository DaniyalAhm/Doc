# App

## Overview

The `App` component serves as the main entry point for the Marketplace Game built using the Empirica framework. It integrates key elements such as the game stages, player forms, introductory steps, and exit surveys. This component utilizes Empirica's `EmpiricaParticipant` and `EmpiricaContext` to manage the flow of the game from player creation to the conclusion of the experiment.

## Table of Contents

- [Component Structure](#component-structure)
- [Functions](#functions)
  - [introSteps](#introsteps)
  - [exitSteps](#exitsteps)
- [Empirica Integration](#empirica-integration)
- [Styling](#styling)

---

## Component Structure

The `App` component is structured around Empirica’s player management system. It includes a few important sub-components:
- **`EmpiricaParticipant`**: Manages player connection and communication with the Empirica backend.
- **`EmpiricaMenu`**: Displays a menu for debugging or participant options.
- **`EmpiricaContext`**: Provides context for the player and handles the lifecycle of the game.

```jsx
import React from "react";
import { EmpiricaClassic } from "@empirica/core/player/classic";
import { EmpiricaContext } from "@empirica/core/player/classic/react";
import { EmpiricaMenu, EmpiricaParticipant } from "@empirica/core/player/react";
import { Game } from "./Game";
import { ExitSurvey } from "./intro-exit/ExitSurvey";
import { MyPlayerForm } from "./stages/MyPlayerForm.jsx";
import { Introduction } from "./intro-exit/Introduction";
```

---

## Functions

### `introSteps`

```js
function introSteps({ game, player }) {
  return [Introduction];
}
```

- **Description**: Specifies the introductory steps for the player, which includes showing the `Introduction` component at the start of the game.
- **Returns**: An array of components to be rendered as introduction steps.

---

### `exitSteps`

```js
function exitSteps({ game, player }) {
  return [ExitSurvey];
}
```

- **Description**: Specifies the exit steps for the player, which includes showing the `ExitSurvey` component at the end of the game.
- **Returns**: An array of components to be rendered as exit steps.

---

## Empirica Integration

### `EmpiricaParticipant`

```jsx
<EmpiricaParticipant url={url} ns={playerKey} modeFunc={EmpiricaClassic}>
```

- **Description**: Initializes the Empirica player session, connecting the player to the experiment using the provided URL and player key.
- **Props**:
  - **`url`**: The backend URL used for querying and managing the player session.
  - **`ns`**: A namespace or player key used to identify the player.
  - **`modeFunc`**: Specifies the gameplay mode (in this case, `EmpiricaClassic`).

### `EmpiricaContext`

```jsx
<EmpiricaContext playerCreate={MyPlayerForm} introSteps={introSteps} exitSteps={exitSteps}>
```

- **Description**: Provides the context for the game, managing the lifecycle of player creation, introduction, game stages, and exit steps.
- **Props**:
  - **`playerCreate`**: The component used to create a new player (in this case, `MyPlayerForm`).
  - **`introSteps`**: The introduction steps for the player.
  - **`exitSteps`**: The exit steps for the player.

### `EmpiricaMenu`

```jsx
<EmpiricaMenu position="bottom-left" />
```

- **Description**: Adds a menu for participants, with options like debugging tools. The `position` prop defines the location of the menu on the screen.

---

## Styling

The component uses a minimal styling approach. The layout is responsive and fills the screen, ensuring the game stages and forms are fully visible.

```html
<div className="h-screen relative">
  <EmpiricaMenu position="bottom-left" />
  <div className="h-full overflow-auto">
    <EmpiricaContext playerCreate={MyPlayerForm} introSteps={introSteps} exitSteps={exitSteps}>
      <Game />
    </EmpiricaContext>
  </div>
</div>
```

- **`h-screen`**: Ensures the component takes up the full screen height.
- **`overflow-auto`**: Enables scrolling for content that overflows the screen height.
```