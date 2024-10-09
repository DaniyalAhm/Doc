# Feedback Component 

## Overview

The `Feedback` component provides feedback for both consumers and producers at the end of a round in the Marketplace Game. Consumers are able to review the products they purchased and challenge warranties, while producers can see their sales performance and whether their product's warranty claims were challenged.

## Table of Contents

- [Component Structure](#component-structure)
- [Functions](#functions)
  - [handleSubmit](#handlesubmit)
  - [ConsumerFeedback](#consumerfeedback)
  - [ProducerFeedback](#producerfeedback)
- [State](#state)
- [Hooks](#hooks)
- [Conditional Rendering](#conditional-rendering)
- [Styling](#styling)

---

## Component Structure

The `Feedback` component renders feedback differently for consumers and producers, based on the player's role. Consumers can review products and provide feedback, while producers are presented with sales information and consumer reviews.

```jsx
import React, { useState } from "react";
import { usePlayer, usePlayers, useGame, useRound } from "@empirica/core/player/classic/react";
import FeedbackCard from "../components/FeedbackCard.jsx";
import { Profile } from "../Profile.jsx";
import "../components/Toast.css";
```

---

## Functions

### `handleSubmit`

```js
function handleSubmit() {
  const review = player.get("review");

  if (player.get("role") === "consumer") {
    player.set("consumerReviews", consumerReviews);
  }
  player.stage.set("submit", true);
}
```

- **Description**: Submits the feedback and updates the player's stage when the consumer or producer completes their feedback.
- **Returns**: `void`

---

### `ConsumerFeedback`

```js
const ConsumerFeedback = () => {
  const producers = players.filter((p) => p.get("role") === "producer")[0];
  const purchasedFrom = player.get("purchasedFrom").slice(-1)[0];
  const purchased = players.filter((p) => purchasedFrom.indexOf(p.get("name")) !== -1);
  return (
    <div id="body-container" className="h-screen w-screen flex flex-col bg-offwhite">
      <Profile />
      <div id="middle-div" className="flex-auto min-h-middle overflow-auto">
        {purchased.length > 0 ? (
          purchased.map((p, index) => (
            <FeedbackCard key={index} whatProducer={p} />
          ))
        ) : (
          <div className="card">You did not purchase any products!</div>
        )}
      </div>
      <button className="nextBtn" onClick={handleSubmit}>Next {">"}</button>
    </div>
  );
};
```

- **Description**: Displays feedback for consumers, allowing them to review products they purchased. Consumers can challenge warranties and provide reviews.
- **Returns**: JSX element

---

### `ProducerFeedback`

```js
const ProducerFeedback = () => {
  const sold = player.get("sold");
  return (
    <div id="body-container" className="h-screen w-screen flex flex-col bg-offwhite">
      <Profile />
      <div id="middle-div" className="flex-auto min-h-middle overflow-auto">
        {sold.slice(-1) === undefined ? (
          <div className="card">Consumer(s) didn't purchase your products.</div>
        ) : (
          <div className="card">
            <div>Products Sold: {sold.slice(-1)[0].length}</div>
            <div>Total Earnings: ${player.get("totalProfits")}</div>
          </div>
        )}
      </div>
      <button className="nextBtn" onClick={handleSubmit}>Next {">"}</button>
    </div>
  );
};
```

- **Description**: Displays sales feedback for producers, showing the number of products sold and total earnings. Producers also receive feedback on whether their warranties were challenged.
- **Returns**: JSX element

---

## State

No local state is used directly in this component, but `usePlayer`, `usePlayers`, and `useGame` hooks are used to fetch relevant data.

---

## Hooks

- **`usePlayer`**: Fetches the current player’s data, including their role, purchases, and reviews.
- **`usePlayers`**: Retrieves the list of players in the game, allowing the component to filter for consumers and producers.
- **`useGame`**: Retrieves game data, including the current treatment settings.
- **`useRound`**: Retrieves information about the current round.

---

## Conditional Rendering

The component dynamically renders either the `ConsumerFeedback` or `ProducerFeedback` based on the player's role:

```jsx
{player.get("role") === "consumer" && <ConsumerFeedback handleSubmit={handleSubmit} />}
{player.get("role") === "producer" && <ProducerFeedback handleSubmit={handleSubmit} />}
```

If a consumer has purchased products, feedback cards for each product are shown. If no products were purchased, a message is displayed instead.

---

## Styling

- **Tailwind CSS** is used for layout and styling.
- Key classes include: `bg-offwhite`, `rounded-3xl`, `text-xs`, `min-h-middle`, and `bg-blue-600`.

```html
<button className="nextBtn self-center rounded-2xl w-40 text-xs bg-blue-600 text-white tablet:w-32 mb-3 py-4">
  Next {">"}
</button>
```

Each feedback section for both consumers and producers is styled with cards and a responsive layout for various screen sizes.
```