
# ProducerChoice 

## Overview

The `ProducerChoice` component is a key part of the Marketplace Game that allows producers to select the quality of the product they want to produce (low or high) and whether to make a warranted claim about the product's quality. The component handles the submission of these choices and updates the player's data accordingly.

## Table of Contents

- [Component Structure](#component-structure)
- [Functions](#functions)
  - [addItem](#additem)
  - [chooseLow](#chooselow)
  - [chooseHigh](#choosehigh)
  - [selectWarrant](#selectwarrant)
  - [deselectWarrant](#deselectwarrant)
  - [handleSubmit](#handlesubmit)
- [State](#state)
- [Hooks](#hooks)
- [Conditional Rendering](#conditional-rendering)
- [Styling](#styling)

---

## Component Structure

The `ProducerChoice` component is designed for producers in the game to make decisions about their product and its quality. It allows producers to:
1. Select either a **low-quality** or **high-quality** product.
2. Choose to **warrant** the quality of the product.
3. Submit these decisions for the current game stage.

```jsx
import React, { useState, useEffect } from "react";
import { ToastContainer, toast, Zoom } from "react-toastify";
import { usePlayer, useGame } from "@empirica/core/player/classic/react";
import { Profile } from "../Profile.jsx";
import PayoffMatrix from "../components/Payoff.jsx";
```

---

## Functions

### `addItem`

```js
const addItem = (newItem) => {
  setItems((items) => [...items, newItem]);
  player.set("productQuality", [...items, newItem]);
};
```

- **Description**: Adds the selected product quality to the player's `productQuality` array and updates the player's state.
- **Returns**: `void`

---

### `chooseLow`

```js
const chooseLow = () => {
  if (currentQuality !== "low") {
    toast.info("You chose to produce a low quality product!");
  }
  setCurrentQuality("low");
};
```

- **Description**: Sets the product quality to **low** and shows a notification.
- **Returns**: `void`

---

### `chooseHigh`

```js
const chooseHigh = () => {
  if (currentQuality !== "high") {
    toast.info("You chose to produce a high quality product!");
  }
  setCurrentQuality("high");
};
```

- **Description**: Sets the product quality to **high** and shows a notification.
- **Returns**: `void`

---

### `selectWarrant`

```js
const selectWarrant = () => {
  if (!warrantStatus) {
    toast.info("You chose to warrant the claim of your high product quality!");
  }
  setWarrantStatus(true);
  setClickCounter(clickCounter + 1);
};
```

- **Description**: Sets the warrant status to `true`, indicating that the producer warrants the product's quality.
- **Returns**: `void`

---

### `deselectWarrant`

```js
const deselectWarrant = () => {
  if (warrantStatus) {
    toast.info("You chose to not warrant the claim of your high product quality!");
  }
  setWarrantStatus(false);
  setClickCounter(clickCounter + 1);
};
```

- **Description**: Sets the warrant status to `false`, indicating that the producer does not warrant the product's quality.
- **Returns**: `void`

---

### `handleSubmit`

```js
const handleSubmit = () => {
  if (currentQuality === "low") {
    player.set("capital", player.get("capital") - lowQualProdCost);
  } else if (currentQuality === "high") {
    player.set("capital", player.get("capital") - highQualProdCost);
  }

  if (warrantStatus === true) {
    player.set("capital", player.get("capital") - warrantCost);
  }

  if (currentQuality === "") {
    toast.error("Please select a product quality!");
    player.stage.set("submit", false);
    return;
  }

  player.set("currentQuality", currentQuality);
  player.get("warrants").push(warrantStatus);
  player.set("warrants", warrants);

  addItem(player.get("currentQuality"));
  player.stage.set("submit", true);
};
```

- **Description**: Handles the submission of the producer's choices, updating the player's capital based on the product quality and warrant status.
- **Returns**: `void`

---

## State

- **`currentQuality`**: Tracks the quality of the product being produced (either "low" or "high").
- **`warrantStatus`**: Indicates whether the product's quality is warranted.
- **`clickCounter`**: A counter to manage button clicks and styling transitions.
- **`items`**: Stores the list of product qualities selected over time.

---

## Hooks

- **`usePlayer`**: Retrieves the current player's data, including their capital, product quality, and warrant status.
- **`useGame`**: Fetches the current game state, including the number of rounds and any treatments being applied.

---

## Conditional Rendering

The component dynamically renders certain elements based on the player's role and the presence of warrants:
1. **If the Player is a Consumer**: The component immediately submits the stage as the consumer should not see the producer's choice screen.

```js
if (player.get("role") === "consumer") {
  player.stage.set("submit", true);
}
```

2. **Warrant Status Display**: The warrant selection buttons dynamically change color based on the warrant status.

```jsx
<button
  className="bg-blue-600 text-white rounded-2xl p-1.5"
  onClick={selectWarrant}
  style={{
    backgroundColor: warrantStatus ? "rgb(34 197 94)" : "rgb(37 99 235)",
  }}
>
  Yes
</button>
```

---

## Styling

- **Tailwind CSS** is used for styling, making the component responsive and visually appealing.
- Key classes include: `bg-offwhite`, `rounded-3xl`, `text-xs`, `tablet:text-sm`, and `bg-blue-600`.
  
```html
<button
  className="nextBtn self-center rounded-2xl w-40 text-xs bg-blue-600 text-white tablet:w-32 mb-3 py-4"
  onClick={handleSubmit}
>
  Next {">"}
</button>
```

The layout adapts based on the screen size, and buttons are styled with conditional transitions based on the player's selections.
```