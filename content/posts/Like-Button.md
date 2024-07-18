+++
title = 'React Like Button'
date = 2024-01-29T14:35:55+05:30
draft = false
description="Like Button using React and its hooks"
image="/images/1s.webp"
imageBig="/images/1b.webp"
categories=["coding","React","Like Button"]
authors=["MaxDax"]
avatar="/images/images.jpeg"
+++

# Like Button

In this blog lets understand how to implement Like Button using React fundamentals

Concepts that are used in this are :

- useState
- async await
- try/catch block

Lets understand the use of these hooks -->

- useState : Generally useState is mainly used to display the change in UI or store some data whenever an event is triggered by the user.
  In the case of Like button we need three useState cause one for checking whether we have liked or not
  Second to whether we have fetched the icons from the API or not
  Third one is to send the error message if the icons failed to load when called.

```
  const [liked, setLiked] = useState(false);
  const [isFetching, setIsFetching] = useState(false);
  const [error, setError] = useState(null);
```

Icons.js

```
export function SpinnerIcon({ className }) {
  return (
    <svg
      className={className}
      width={16}
      height={16}
      viewBox="0 0 38 38"
      xmlns="http://www.w3.org/2000/svg"
      stroke="currentColor"
    >
      <g fill="none" fillRule="evenodd">
        <g transform="translate(1 1)" strokeWidth="2">
          <circle strokeOpacity=".5" cx="18" cy="18" r="18" />
          <path d="M36 18c0-9.94-8.06-18-18-18">
            <animateTransform
              attributeName="transform"
              type="rotate"
              from="0 18 18"
              to="360 18 18"
              dur="1s"
              repeatCount="indefinite"
            />
          </path>
        </g>
      </g>
    </svg>
  );
}

export function HeartIcon({ className }) {
  return (
    <svg
      className={className}
      fill="currentColor"
      viewBox="0 0 24 24"
      width="16"
      height="16"
    >
      <g>
        <path d="M12 21.638h-.014C9.403 21.59 1.95 14.856 1.95 8.478c0-3.064 2.525-5.754 5.403-5.754 2.29 0 3.83 1.58 4.646 2.73.814-1.148 2.354-2.73 4.645-2.73 2.88 0 5.404 2.69 5.404 5.755 0 6.376-7.454 13.11-10.037 13.157H12zM7.354 4.225c-2.08 0-3.903 1.988-3.903 4.255 0 5.74 7.034 11.596 8.55 11.658 1.518-.062 8.55-5.917 8.55-11.658 0-2.267-1.823-4.255-3.903-4.255-2.528 0-3.94 2.936-3.952 2.965-.23.562-1.156.562-1.387 0-.014-.03-1.425-2.965-3.954-2.965z"></path>
      </g>
    </svg>
  );
}

```

The above code holds the two icons one is HeartIcon and second one is SpinnerIcon which is like a loading icons which is gonna get displayed when we are not able to fetch the HeartIcon from the API

- try/catch block : It is mainly used in some function where we are checking the status of the API response if we get any response it moves to try block and runs a piece of code or if we dont get any response from the API then it runs catch block code.
  Mainly used for error handling

- Use of async/await : As we are using a third party API to fetch the data of the products so it will take time to return the response so dont wanna stop the execution of the remaining code so we make this task asynchronous by using async/await . There is one more away of dealing this that use of promise .then and .catch
- To fetch the API we used inbuilt method ie is fetch() there is another way of doing it by installing a third party library known as axios which is better then fetch as its error handling capabilities are better.
  In this we are using to fetch the response once we post into it .
  You will get to know once you go through the code

Lets understand onClick function that the button has which has the main logic of the blog

```
const handleLikeUnlike = async () => {
    setError(null);
    setIsFetching(true);

    try {
      const response = await fetch(
        "https://www.greatfrontend.com/api/questions/like-button",
        {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          //why stringify learn about
          body: JSON.stringify({
            action: liked ? "unlike" : "like",
          }),
        }
      );

      if (response.status >= 200 && response.status < 300) {
        setLiked(!liked);
      } else {
        const res = await response.json();
        setError(res.message);
        return;
      }
    } finally {
      setIsFetching(false);
    }
  };

```

- The above code explains that once you enter the onClick function which is asynchronous function cause we are doing POST method to the API we set the setError to null **idk the reason learn about it** and setIsFetching to true cause we are gonna do PUT request to the API for the required data/icon
  So now to check whether we are getting 202 status or 404 status we are gonna use try/catch block inside the try block we are doing PUT request to the API endpoint and sending the data in string format which is the standard format the body we are sending is
  if liked is true make it unlike and vice versa **why idk need to look into it **

Later if the we are getting the desired response we go into if block and toggle the setLiked so that when user clicks on liked button it goes to unlike and vice versa
But if we get any error response from API we are gonna return the error message in setError which is put that in error variable from useState hook and later we can display it in our webpage

Finally if nothing gets satisfied that is no try/catch block the finally block is gonna run and inside it we are setting setIsFetching(false) so that we can display message that we are not able to fetch the icons

- Now lets see how to display this in our webpage

```
<div>
      <button
        disabled={isFetching}
        className={`likeBtn ${liked ? "liked" : ""}`}
        onClick={handleLikeUnlike}
      >
        {isFetching ? <SpinnerIcon /> : <HeartIcon />}{" "}
        {liked ? "Liked" : "Like"}
      </button>
      {error && <div className="error">{error}</div>}
</div>
```

- The button holds the HeartIcon and SpinnerIcon and it conditionally renders it using a ternary operator and to change the style of it again we are gonna use the ternary operator in className and later to avoid sudden click b/w like to liked we are gonna use disabled attribute based on isFetching as it return true or falses

- Later if we dont get isFetching to false we are gonna return the error message that we got from API response

Style.css for this project :

```
:root {
  --default-color: #888;
  --active-color: #f00;
}

* {
  font-family: sans-serif;
}

.likeBtn {
  border: 2px solid var(--default-color);
  display: flex;
  align-items: center;
  gap: 8px;
  border-radius: 32px;
  cursor: pointer;
  font-weight: bold;
  height: 30px;
  padding: 4px 8px;
  margin-bottom: 5px;
  color: var(--default-color);
  background-color: #fff;
}

.likeBtn:hover {
  border-color: var(--active-color);
  color: var(--active-color);
}

.liked,
.liked:hover {
  background-color: var(--active-color);
  border-color: var(--active-color);
  color: #fff;
}

.likeBtn-icon {
  display: flex;
}

.error {
  font-size: 12px;
}

```

That's it about Like Button there is still room for imporvement and we can add some more functionality into
For that we will see in next blog

Stay Tuned
MaxDax
