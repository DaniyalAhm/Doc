
# SelectRole 
## Overview

The `SelectRole` component serves as the introductory stage in the Marketplace Game, where players are introduced to their roles—either "Producer" or "Consumer". This component displays specific instructions for each role, including the game's objectives, starting capital, or wallet, and key gameplay mechanics. It allows players to submit their role selection and start the game.

## Table of Contents

- [Component Structure](#component-structure)
- [Functions](#functions)
  - [handleSubmit](#handlesubmit)
  - [ProducerInfo](#producerinfo)
  - [ConsumerInfo](#consumerinfo)
- [Hooks](#hooks)
- [Styling](#styling)

---

## Component Structure

The `SelectRole` component determines the player's role (producer or consumer) and renders role-specific information. The instructions are displayed in card format, and a button is provided to start the game.

```jsx
import React, { useState } from "react";
import { Button } from "../components/Button";
import { usePlayer, useStage, useRound, useGame } from "@empirica/core/player/classic/react";
import { Profile } from "../Profile";
import { productPrice, warrantCost, lowQualProdCost, highQualProdCost, initialCapital, initialWallet } from "../constants.js";
```

---

## Functions

### `handleSubmit`

```js
function handleSubmit() {
  if (round.get("name") == "Round1") {
    game.set("gameData", {
      highQualProdCost: highQualProdCost,
      lowQualProdCost: lowQualProdCost,
      productPrice: productPrice,
      warrantCost: warrantCost,
      round: 1,
    });

    if (player.get("role") === "producer") {
      player.set("producerData", {
        id: player.id,
        role: "producer",
        capital: 0,
        score: 0,
        currentQuality: "",
        productQuality: [],
        brandName: "",
        reviews: [0, 0],
        profits: 0,
        warrants: [],
        claims: [0, 0],
        sold: [],
      });
    } else if (player.get("role") === "consumer") {
      player.set("consumerData", {
        id: player.id,
        role: "consumer",
        wallet: 0,
        score: 0,
        boughtStock: false,
        cheatedHistory: [],
        challengedWarrant: false,
        changedBrand: false,
        brandName: "",
      });
    }
  } else {
    console.log("Error: Role not selected!");
  }
  player.stage.set("submit", true);
}
```

- **Description**: This function sets the initial game and player data based on the role (producer or consumer) for the first round and submits the player's stage when they are ready.
- **Returns**: `void`

---

### `ProducerInfo`

```js
function ProducerInfo() {
  return (
    <div id="intro-container" className="h-svh flex flex-col justify-center items-center xs:mt-6">
      <div id="intro-card-container" className="card rounded-3xl flex flex-col p-12 xs:p-9 justify-items-center xl:w-maxintroCardW xl:h-maxintroCardH lg:w-maxintroCardW lg:h-maxintroCardH md:w-maxintroCardW md:h-maxintroCardH xs:w-26 m-14">
        <div className="text-center">
          <p className="text-xl text-blue-600 desktoplg:mb-10 tablet:mb-10 xs:my-3">Welcome Producer!</p>

          <div className="text-xs desktoplg:text-sm desktop:text-sm tablet:text-sm border-2 border-blue-600 rounded-3xl mb-5 p-2 relative">
            <div className="bg-blue-600 text-white text-md rounded-full absolute top-[-15px] left-[-10px] py-1 px-3">1</div>
            Dive into the world of the Marketplace Game! As a producer, your quest is to skyrocket your profits and climb the ladder to wealth.
          </div>

          <div className="text-xs desktoplg:text-sm desktop:text-sm tablet:text-sm border-2 border-blue-600 rounded-3xl my-5 p-2 relative">
            <div className="bg-blue-600 text-white text-md rounded-full absolute top-[-15px] left-[-10px] py-1 px-3">2</div>
            You will start with ${initialCapital} in venture capital. Implement strategy to maximize earnings.
          </div>

          <div className="text-xs desktoplg:text-sm desktop:text-sm tablet:text-sm border-2 border-blue-600 rounded-3xl my-5 p-2 relative">
            <div className="bg-blue-600 text-white text-md rounded-full absolute top-[-15px] left-[-10px] py-1 px-3">3</div>
            🚀 Are you ready to rise to the occasion and claim your title as the ultimate producer?
          </div>
        </div>
      </div>
      <div className="mb-5">
        <button id="nextBtn" className="rounded-2xl bg-blue-600 text-white text-xs p-4 mb-3" onClick={handleSubmit}>I'm Ready!</button>
      </div>
    </div>
  );
}
```

- **Description**: Renders information and instructions specific to the "Producer" role. Provides a button for the player to submit when they are ready.
- **Returns**: JSX element

---

### `ConsumerInfo`

```js
function ConsumerInfo() {
  return (
    <div id="intro-container" className="h-svh flex flex-col justify-center items-center">
      <div id="intro-card-container" className="card rounded-3xl flex flex-col p-12 xs:p-8 justify-items-center xl:w-maxintroCardW xl:h-maxintroCardH lg:w-maxintroCardW lg:h-maxintroCardH md:w-maxintroCardW md:h-maxintroCardH xs:w-26 m-14">
        <div className="text-center">
          <p className="text-xl text-blue-600 desktoplg:mb-10 tablet:mb-10 xs:my-1 relative">Welcome Consumer!</p>

          <div className="text-xs desktoplg:text-sm desktop:text-sm tablet:text-sm border-2 border-blue-600 rounded-3xl mb-5 p-3 relative">
            <div className="bg-blue-600 text-white text-md rounded-full absolute top-[-15px] left-[-10px] py-1 px-3">1</div>
            Step into the Marketplace Game! As a consumer, your mission is to make wise purchases from producers based on their behavior.
          </div>

          <div className="text-xs desktoplg:text-sm desktop:text-sm tablet:text-sm border-2 border-blue-600 rounded-3xl my-5 p-3 relative">
            <div className="bg-blue-600 text-white text-md rounded-full absolute top-[-15px] left-[-10px] py-1 px-3">2</div>
            You will start with ${initialWallet}. The real quality of each product will be revealed after your purchases.
          </div>

          <div className="text-xs desktoplg:text-sm desktop:text-sm tablet:text-sm border-2 border-blue-600 rounded-3xl my-3 p-2 relative">
            <div className="bg-blue-600 text-white text-md rounded-full absolute top-[-15px] left-[-10px] py-1 px-3">3</div>
            🌟 Are you ready to emerge as the ultimate consumer?
          </div>
        </div>
      </div>
      <div className="mb-5">
        <button id="nextBtn" className="rounded-2xl bg-blue-600 text-white text-xs p-4 mb-3" onClick={handleSubmit}>I'm Ready!</button>
      </div>
    </div>
  );
}
```

- **Description**: Renders information and instructions specific to the "Consumer" role. Provides a button for the player to submit when they are ready.
- **Returns**: JSX element

---

## Hooks

- **`usePlayer`**: Retrieves the current player's data, such as their role.
- **`useStage`**: Retrieves the current stage information.
- **`useRound`**: Retrieves information about the current round.
- **`useGame`**: Retrieves the game data, including treatments and general configurations.

---

## Styling

- **Tailwind CSS** is used for layout and design.
- Key classes used include `h-svh`, `w-screen`, `flex`, `bg-offwhite`, `rounded-3xl`, `bg-blue-600`, and responsive classes such as `desktoplg:text-sm`, `xs:mt-6`, and `p-12`.

```html
<button id="nextBtn" className="rounded-2xl bg-blue-600 text-white text-xs p-4 mb-3">
  I'm Ready!
</button>
```

The overall layout is structured with profile information at the top and a card-based display of role instructions for producers and consumers.
```