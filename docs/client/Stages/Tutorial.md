# Tutorial

## Overview

The `Tutorial` component provides role-based instructions for both consumers and producers in the Marketplace Game. Depending on the player's role, either the `ConsumerTutorial` or `ProducerTutorial` is displayed. The tutorial walks the player through different stages of the game and includes navigation buttons to switch between tutorial pages.

## Table of Contents

- [Component Structure](#component-structure)
- [Functions](#functions)
  - [handleSubmit](#handlesubmit)
  - [ConsumerTutorial](#consumertutorial)
  - [ProducerTutorial](#producertutorial)
- [State](#state)
- [Hooks](#hooks)
- [Styling](#styling)

---

## Component Structure

The `Tutorial` component conditionally renders either the `ConsumerTutorial` or `ProducerTutorial` based on the player's role. Each tutorial consists of multiple pages with instructions, navigation buttons to move between pages, and a submit button to proceed.

```jsx
import React, { useState } from "react";
import { usePlayer } from "@empirica/core/player/classic/react";
import TutorialCard from "../components/TutorialCard.jsx";
```

---

## Functions

### `handleSubmit`

```js
const handleSubmit = () => {
  player.set("boughtStock", false);
  player.stage.set("submit", true);
};
```

- **Description**: Marks the player's stage as submitted and resets the stock purchase status to `false`.
- **Returns**: `void`

---

### `ConsumerTutorial`

```js
const ConsumerTutorial = () => {
  const nextPage = () => setPage((prevPage) => (prevPage % 4) + 1);
  const prevPage = () => setPage((prevPage) => (prevPage % 4) - 1);

  return (
    <div>
      <div className="text-center mb-2">consumer</div>
      <div id="bottom-div" className="flex flex-col justify-center min-h-bottom tablet:min-h-smBottom bg-transparent">
        {page === 0 && (
          <TutorialCard
            pageNumber={page}
            toggle={nextPage}
            toggleBack={prevPage}
            title="#1 Producers Select a Product Quality"
            subtitle="Producers select a product quality to produce and whether to make a warranted claim about the product's quality."
            imageSrc={"./images/ProducerPayoff1.png"}
          />
        )}
        {page === 1 && (
          <TutorialCard
            pageNumber={page}
            toggle={nextPage}
            toggleBack={prevPage}
            title="#2 Consumers Purchase Products"
            subtitle="Consumers are shown a set of products to purchase. They can make their decision based on reputation or warranted product claims."
            imageSrc={"./images/ConsumerChoice.png"}
          />
        )}
        {page === 2 && (
          <>
            <TutorialCard
              pageNumber={page}
              toggle={nextPage}
              toggleBack={prevPage}
              title="#3 Consumers Rate Producers and Challenge Warrants"
              subtitle="Consumers are given the opportunity to rate the producer or challenge the claim on the product's quality."
              imageSrc={"./images/ConsumerFeedback.png"}
            />
            <button
              id="bottom-div"
              className="self-center rounded-2xl w-40 text-xs bg-blue-600 text-white tablet:w-32 mb-3 py-4"
              onClick={handleSubmit}
            >
              Next {">"}
            </button>
          </>
        )}
      </div>
    </div>
  );
};
```

- **Description**: Displays a multi-page tutorial for consumers, providing instructions on purchasing products, rating producers, and challenging warrants.
- **Returns**: JSX element

---

### `ProducerTutorial`

```js
const ProducerTutorial = () => {
  const nextPage = () => setPage((prevPage) => (prevPage % 4) + 1);
  const prevPage = () => setPage((prevPage) => (prevPage % 4) - 1);

  return (
    <div>
      <div className="text-center mb-2">producer</div>
      <div id="bottom-div" className="flex flex-col justify-center min-h-bottom tablet:min-h-smBottom bg-transparent">
        {page === 0 && (
          <TutorialCard
            pageNumber={page}
            toggle={nextPage}
            toggleBack={prevPage}
            title="#1 Producers Select a Product Quality"
            subtitle="Producers select a product quality to produce and whether to make a warranted claim about the product's quality."
            imageSrc={"./images/ProducerChoice.png"}
          />
        )}
        {page === 1 && (
          <TutorialCard
            pageNumber={page}
            toggle={nextPage}
            toggleBack={prevPage}
            title="#2 Consumers Rate Producers and Challenge Warrants"
            subtitle="Consumers rate the producer and challenge the claim on the product's quality."
            imageSrc={"./images/ConsumerFeedback.png"}
          />
        )}
        {page === 2 && (
          <>
            <TutorialCard
              pageNumber={page}
              toggle={nextPage}
              toggleBack={prevPage}
              title="#3 Producers Are Given a Chance to Change Brands"
              subtitle="Producers are shown feedback and given the opportunity to change their brand."
              imageSrc={"./images/ProducerResults.png"}
            />
            <button
              id="bottom-div"
              className="self-center rounded-2xl w-40 text-xs bg-blue-600 text-white tablet:w-32 mb-3 py-4"
              onClick={handleSubmit}
            >
              Next {">"}
            </button>
          </>
        )}
      </div>
    </div>
  );
};
```

- **Description**: Displays a multi-page tutorial for producers, providing instructions on selecting product quality, receiving consumer feedback, and changing brands.
- **Returns**: JSX element

---

## State

- **`page`**: The current page of the tutorial, controlled using the `useState` hook. This is used to navigate between different sections of the tutorial.

---

## Hooks

- **`usePlayer`**: Fetches the current player data, including their role (consumer or producer).

---

## Styling

- The component uses Tailwind CSS for styling.
- Key classes include: `text-center`, `rounded-2xl`, `bg-blue-600`, `text-white`, `w-40`, `tablet:w-32`, `mb-3`, `py-4`.
  
```html
<button
  id="bottom-div"
  className="self-center rounded-2xl w-40 text-xs bg-blue-600 text-white tablet:w-32 mb-3 py-4"
  onClick={handleSubmit}
>
  Next {">"}
</button>
```

- **Conditional Rendering**: Based on the player’s role, either `ConsumerTutorial` or `ProducerTutorial` is rendered.

```jsx
<div>
  {player.get("role") === "consumer" && ConsumerTutorial()}
  {player.get("role") === "producer" && ProducerTutorial()}
</div>
```
```