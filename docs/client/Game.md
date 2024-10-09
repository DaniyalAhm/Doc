
# Game 

## Overview

The `Game` component is the main interface for the Marketplace Game. It is responsible for rendering the current stage of the game and (optionally) a chat interface if the game involves multiple players. The component uses Empirica’s `useGame` hook to fetch game data, such as the number of players.

## Table of Contents

- [Component Structure](#component-structure)
- [Hooks](#hooks)
- [Conditional Rendering](#conditional-rendering)
- [Styling](#styling)

---

## Component Structure

The `Game` component renders the current game stage using the `Stage` component. It also includes optional chat functionality, which is only displayed if the game has more than one player.

```jsx
import { Chat, useGame } from "@empirica/core/player/classic/react";
import React from "react";
import { Profile } from "./Profile";
import { Stage } from "./Stage";
```

---

## Hooks

### `useGame`

```js
const game = useGame();
```

- **Description**: This hook retrieves the current game state, including game settings and treatments. It allows access to game data such as the number of players and the treatment being applied.
- **Returns**: An object containing game-related data.

---

## Conditional Rendering

### Chat Visibility

The `Game` component conditionally renders a chat window based on the number of players in the game. If there is more than one player, the chat interface will be displayed.

```jsx
{playerCount > 1 && (
  <div className="h-full w-[32rem] border-l flex justify-center items-center">
    <Chat scope={game} attribute="chat" />
  </div>
)}
```

- **`playerCount`**: This variable is retrieved from the game's treatment settings to determine if the chat should be shown. If the player count is greater than 1, the chat is rendered.

---

## Styling

The component uses Tailwind CSS for styling. The layout is designed to be responsive and adjust based on screen size, ensuring the stage and chat components are well-positioned on the screen.

### Main Layout

```html
<div className="h-full w-full flex">
  <div className="h-full w-full flex flex-col">
    <div className="h-full flex items-center justify-center">
      <Stage />
    </div>
  </div>

  {playerCount > 1 && (
    <div className="h-full w-[32rem] border-l flex justify-center items-center">
      <Chat scope={game} attribute="chat" />
    </div>
  )}
</div>
```

- **`h-full`**: Ensures the component takes up the full height of the screen.
- **`w-full`**: Ensures the component takes up the full width of the screen.
- **`flex`**: Applies Flexbox to align items, making the layout flexible and responsive.
- **`border-l`**: Adds a left border to visually separate the chat window from the game stage.

---

## Optional Features

- **Chat**: The chat functionality can be enabled or disabled based on the game's player count. When enabled, it provides a communication channel between players.
```