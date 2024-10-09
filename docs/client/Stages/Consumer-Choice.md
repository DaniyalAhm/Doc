# ConsumerChoice 
## Overview

The `ConsumerChoice` component allows players with the "consumer" role to decide whether to purchase a product from the producer. It displays relevant information, including a product matrix, instructions, and product cards. The component also handles the process of submitting a consumer's choice. If the player is a producer, the stage is auto-submitted.

## Table of Contents

- [Component Structure](#component-structure)
- [Functions](#functions)
  - [Purchase](#purchase)
- [State](#state)
- [Hooks](#hooks)
- [Conditional Rendering](#conditional-rendering)
- [Styling](#styling)

---

## Component Structure

The `ConsumerChoice` component renders a layout that allows consumers to view product options from producers and decide whether to make a purchase. The interface includes:
- **Profile Section**: Displays the current player's profile at the top.
- **Instructions Section**: Provides instructions and displays a utility matrix (or payoff matrix) to help consumers make a choice.
- **Product Card Section**: Displays the available products from the producers.
- **Submit Button**: A button to finalize the consumer's choice and submit it.

```jsx
import React, { useState, useEffect } from "react";
import { ToastContainer } from "react-toastify";
import { usePlayer, usePlayers, useGame } from "@empirica/core/player/classic/react";
import { Profile } from "./../Profile.jsx";
import ProductCard from "../components/ProductCard.jsx";
import { productPrice } from "../constants.js";
import PayoffMatrix from "../components/Payoff.jsx";
```

---

## Functions

### `Purchase`

```js
function Purchase() {
  player.stage.set("submit", true);
}
```

- **Description**: Handles the submission of the consumer's purchase decision by setting the `submit` status to `true` for the current stage.
- **Returns**: `void`

---

## State

No local state is managed directly within this component, but it relies on hooks to retrieve the current player, players in the game, and game data.

---

## Hooks

- **`usePlayer`**: Retrieves the current player’s data, such as their wallet and role (producer or consumer).
- **`usePlayers`**: Retrieves a list of all players in the game. This is used to filter and display the producers.
- **`useGame`**: Fetches the game state, including treatments and round configurations.

---

## Conditional Rendering

- **If the Player is a Producer**: The component will auto-submit the stage if the player’s role is "producer". This is handled early in the logic:

```js
if (player.get("role") === "producer") {
  player.stage.set("submit", true);
}
```

- **Product Display**: If producers have set a product quality, the product cards will be displayed. If not, a message is shown indicating that no products were sold this round.

```jsx
{producer.get("productQuality").slice(-1)[0] !== "" ? (
  <div id="middle-div">
    {producersArray.map((productCard, index) => (
      <ProductCard
        key={index}
        imagePath={imagePath}
        productPrice={productPrice}
        whatProducer={producersArray[index]}
        wallet={player.get("wallet")}
        boughtStock={player.get("boughtStock")}
        Purchase={Purchase}
      />
    ))}
  </div>
) : (
  <div id="middle-div">
    <p>Sorry, the producer did not sell anything this round.</p>
  </div>
)}
```

---

## Styling

- **Tailwind CSS** is used for layout and design.
- The component makes use of classes such as `h-screen`, `w-screen`, `bg-offwhite`, `rounded-3xl`, `text-xs`, and `bg-blue-600` for its responsive layout and card design.
  
```html
<div id="body-container" className="h-screen w-screen flex flex-col bg-offwhite">
  <div id="top-div" className="min-h-top flex justify-center">
    <Profile />
  </div>
  ...
</div>
```

- **Product Cards**: The component includes dynamic rendering of the `ProductCard` component for each producer.

```jsx
<ProductCard
  imagePath={imagePath}
  productPrice={productPrice}
  whatProducer={producersArray[index]}
  wallet={player.get("wallet")}
  boughtStock={player.get("boughtStock")}
  Purchase={Purchase}
/>
```

- **Payoff Matrix**: The utility matrix is displayed based on whether the warrant treatment is active. This is conditionally rendered within the instruction card.

```jsx
<div className="w-auto h-auto">
  {warrantTreatment === true ? (
    <PayoffMatrix role="consumer" warranted="True" />
  ) : (
    <img
      src="./images/pay_off_matrix-consumer3.png"
      alt="payoffmatrix"
      className="w-full h-auto"
    />
  )}
</div>
```

- **Submit Button**: A styled button to submit the consumer's decision.

```html
<button
  className="nextBtn self-center rounded-2xl w-40 text-xs bg-blue-600 text-white tablet:w-32 mb-3 py-4"
  onClick={() => Purchase()}
>
  Next {`>`}
</button>
```
```