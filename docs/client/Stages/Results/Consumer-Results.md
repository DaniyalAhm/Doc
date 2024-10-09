```markdown
# ConsumerResults Component Documentation

## Overview

The `ConsumerResults` component is responsible for displaying the results specific to a consumer in the game. It shows details such as the products purchased from producers, the total score, and allows consumers to review their performance in the game.

## Table of Contents

- [Component Structure](#component-structure)
- [Functions](#functions)
  - [handleSubmit](#handlesubmit)
- [State](#state)
- [Hooks](#hooks)
- [Styling](#styling)

---

## Component Structure

The `ConsumerResults` component consists of two main sections:
1. **Purchased Products Summary**: Displays the products purchased by the consumer from different producers.
2. **Score Summary**: Shows the consumer's total score based on their purchases.

```jsx
const ConsumerResults = () => {
  const player = usePlayer();
  const players = usePlayers();
  const purchasedFrom = player.get("purchasedFrom").slice(-1)[0];
  const purchased = players.filter((p) => purchasedFrom.indexOf(p.get("name")) !== -1);
  const score = player.get("score");

  return (
    <div className="h-screen w-screen flex flex-col bg-offwhite">
      <div className="min-h-top flex justify-center">
        <Profile />
      </div>

      <div className={purchased.length > 0 ? "grid grid-cols-1 tablet:grid-cols-2 desktoplg:grid-cols-4" : "flex justify-center"}>
        {purchased.length > 0 ? (
          purchased.map((product, index) => (
            <ResultsCard key={index} imagePath={"./images/bag.png"} whatProducer={product} productPrice={productPrice} />
          ))
        ) : (
          <div className="card min-h-[300px] max-h-[500px] min-w-[350px] max-w-[350px] rounded-3xl text-center p-6">
            <p>You did not purchase any products!</p>
            <p>Your current total score: {score.reduce((partialSum, a) => partialSum + a, 0)}</p>
          </div>
        )}
      </div>

      <div className="flex justify-center">
        <button onClick={handleSubmit}>Next {">"}</button>
      </div>
    </div>
  );
};
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

- **Description**: Submits the consumer's results and moves to the next stage.
- **Returns**: `void`

---

## State

No local state is maintained in this component. It relies on data from the player's and producers' objects.

---

## Hooks

- **`usePlayer`**: Retrieves the current player data, including score and purchased products.
- **`usePlayers`**: Retrieves all players in the game, used to filter the producers and map the purchased products.

---

## Styling

- The component uses Tailwind CSS classes for styling.
- Key classes include: `h-screen`, `w-screen`, `flex`, `justify-center`, `bg-offwhite`, `rounded-3xl`, `text-center`, `p-6`.
- Conditional rendering is used to dynamically adjust the layout based on whether the consumer has purchased any products or not.
```