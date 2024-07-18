+++
title = 'React Grid Light'
date = 2024-01-29T15:25:55+05:30
draft = false
description="Grid Light using React and its hooks"
image="/images/1s.webp"
imageBig="/images/1b.webp"
categories=["coding","React","Grid Light"]
authors=["MaxDax"]
avatar="/images/images.jpeg"
+++

# Grid Lights

In this blog lets understand how to implement Grid Lights using React fundamentals

Concepts that are used in this are :

- useState
- setInterval

Lets understand the use of these hooks -->

- useState : Generally useState is mainly used to display the change in UI or store some data whenever an event is triggered by the user.
- In the case of Grid Lights useState is used track the order the we have clicked on the div and to check whether the div is deactivating or not ie whether the div light is on not

```
  const [order, setOrder] = useState([]);
  const [isDeactivating, setIsDeactivating] = useState(false);
```

- setInterval:Generally it is used to perform some asynchronous function like counter display or something
  In this case it is used to change the color after every 300ms . You will understand once you go through code.

- Now to make the configuration of the div make it modular we can use array format ie

```
  const config = [
    [1, 1, 1],
    [1, 0, 1],
    [1, 1, 1],
  ];
```

- The above code makes it easier to change the number of grid and it also helps you in making empty space between the grid isnt this interesting !!!

- Lets see how to display this in webpage :

```
<div className="wrapper">
      <div
        className="grid"
        style={{ gridTemplateColumns: `repeat(${config[0].length},1fr)`}}>
        {config.flat(1).map((item, index) => {
          return item ? (
        <Cell
              key={index}
              filled={order.includes(index)}
              onClick={() => activateCells(index)}
              isDisable={order.includes(index) || isDeactivating}
        />
          ) : (
        <span />
          );
        })}
      </div>
</div>
```

- The above code actually displays the grid but cause the config is a 2-D array we are gonna use .flat() method to convert 2-D array to 1-D array and later we can map through the array and if the array contains value of one then we are gonna go through the cell component and display the grid and if we get the value of map as zero then we are gonna display an empty span which makes empty space between the cell/grid

- The cell component takes 3 things as props which is filled , onClick function and isDisable which helps you build grid light

- Lets see whats there in Cell component

```
const Cell = ({ filled, onClick, isDisable }) => {
  return (
    <button
      type="button"
      disabled={isDisable}
      onClick={onClick}
      className={filled ? "cell cell-activated" : "cell"}
    />
  );
};
```

- The above code takes the props value which helps in designing of button .
  The button color is conditionally rendered using ternary operator with className with filled as props and to avoid multiple clicks b/w operation we have used disabled attribute and once clicked on button it runs onClick function and performs the required functionality which it got as a props

- Lets understand the logic of onClick function :

```
  function activateCells(index) {
    const newOrder = [...order, index];
    setOrder(newOrder);

    //deactivate and learn what is this if condition learn about it
    if (newOrder.length === config.flat(1).filter(Boolean).length) {
      deactivateCells();
    }
  }
```

- The above function runs whenever one of the button is clicked and it takes index of the button as a parameter and based on the it maintains order in which we have clicked onto the button
  This new order is then passed into the setOrder() which in turn passes onto order variable of the useState hook

- Now if the newOrder length is equal to length of config then run the deactivateCells() function which is gonna turn the color of button from green to transparent in reverse order

```
config.flat(1).filter(Boolean).length)
```

** Learn why we are doing this like .filter(Boolean) ? **

-Now lets see deactivateCells function :

```
  function deactivateCells() {
    setIsDeactivating(true);
    const timer = setInterval(() => {
      setOrder((orginOrder) => {
        const newOrder = orginOrder.slice();
        newOrder.pop();

        if (newOrder.length === 0) {
          clearInterval(timer);
          setIsDeactivating(false);
        }

        return newOrder;
      });
    }, 300);
  }
```

- The above code is mainly changes the setIsDeactivating() to true so that change of color happens and we want this to happens in every 300 ms so we are gonna use setInterval() method for it

- The logic if making the color change is first we are gonna change the setOrder and change its pervious value and use slice method on top of it to make the number comma seperated so that we can easily use .pop() method and remove the newOrder and come back to orginalOrder that is to config array
  -Now to stop this setInterval we are gonna use clearInterval and setIsDeactivating to false when the newOrder length is equal to zero

Style.css file :

```
body {
  font-family: sans-serif;
}

.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 16px;
}

.grid {
  display: grid;
  max-width: 300px;
  width: 100%;
  padding: 20px;
  gap: 20px;
  border: 1px solid #000;
}

.cell {
  background-color: transparent;
  border: 1px solid #000;
  height: 0;
  padding-bottom: 100%;
}
.cell-activated {
  background-color: green;
}

```

That's it about grid light there is better way of doing which is gonna be my next blog

Stay tuned
MaxDax
