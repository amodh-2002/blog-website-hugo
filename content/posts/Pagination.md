+++
title = 'React Pagination'
date = 2024-01-22T11:36:55+05:30
draft = false
description="Pagination using React and its hooks"
image="/images/1s.webp"
imageBig="/images/1b.webp"
categories=["coding","React","Pagination"]
authors=["MaxDax"]
avatar="/images/images.jpeg"
+++

# Pagination Using React

In this blog lets understand how to implement pagination using React fundamentals

Concepts that are used in this are :

- useState
- useEffect
- async await

Lets understand the use of these hooks -->

- useState : Generally useState is mainly used to display the change in UI or store some data whenever an event is triggered by the user.
- In the case of pagination this hook is mainly used to handle the products part as well as change of pages cause whenever the user clicks on page-2 button different products needs to displayed so it completely makes sense to use useState hook to the pages button
  similarly as soon as you reach to some new page new products needs to be displayed so adding a state to products list also makes sense

```
const [products, setProducts] = useState([]);
const [pages, setPages] = useState(1);
```

- Use of async/await : As we are using a third party API to fetch the data of the products so it will take time to return the response so dont wanna stop the execution of the remaining code so we make this task asynchronous by using async/await . There is one more away of dealing this that use of promise .then and .catch
- To fetch the API we used inbuilt method ie is fetch() there is another way of doing it by installing a third party library known as axios which is better then fetch as its error handling capabilities are better.

```
const fetchProducts = async () => {
    const res = await fetch("https://dummyjson.com/products?limit=100");
    const data = await res.json();
    if (data && data.products) {
      setProducts(data.products);
    }
    // console.log(data);
  };
  console.log(products);
```

The above code fetchs the data from the API and stores it in res variable but it is not in required format so we use .json() method on it so that we can extract data in object format as it is better way of getting the data
Once we are sure that we are getting data and the data.products the push the data.products into setProducts which in turn pushes the data.products into products variable of useState hook so we can later do some manipulation on it and display the products onto our website

- useEffect: Generally useEffect is used to perform some side-effects in our website like displaying the fetched API data as soon as the website loads or do some changes in UI when certain part of UI changes .
- In the case of pagination useEffect is used to initially display ie render only when the website loads initially the products that we fetched from the API

```
 useEffect(() => {
    fetchProducts();
  }, []);
```

In the above code the useEffect is just calling the fetchProducts function as soon as the page loads cause we have kept dependency array [] as empty

First lets display the products that we have fetched from the API :

```
    {products.length > 0 && (
        <div className="products">
          {products.slice(pages * 10 - 10, pages * 10).map((product) => {
            return (
              <span className="products__single" key={product.id}>
                <img src={product.thumbnail} alt={product.title} />
                <span>{product.title}</span>;
              </span>
            );
          })}
        </div>
```

The above code display the product like its title and image before that we make sure that we have enough products in our products array as it is a good way of checking whether products exist or not .
Now to make sure that we display only 10 products per page use .slice() method . Important to know about slice so learn about it properly it is heaveally used in react especially for pagination later to display the products we have used .map() method as product is any array to we can loop through it using .map we cant use forEach cause it doesnt return html tags as such

Next to display the buttons so that we can move from one page to another :

```
    {products.length > 0 && (
        <div className="pagination">
          <span
            className={pages > 1 ? "" : "pagination__disable"}
            onClick={() => selectPageHandler(pages - 1)}
          >
            ←
          </span>
          {
            //below code is important learn about it and see where else we can use it
            [...Array(products.length / 10)].map((_, i) => {
              return (
                <span
                  className={pages === i + 1 ? "pagination__selected" : ""}
                  onClick={() => selectPageHandler(i + 1)}
                  key={i}
                >
                  {i + 1}
                </span>
              );
            })
          }
          <span
            className={
              pages < products.length / 10 ? "" : "pagination__disable"
            }
            onClick={() => selectPageHandler(pages + 1)}
          >
            →
          </span>
        </div>
      )}
```

The above code First of all checks whether we have any products in our array or not if products exsist to displays the buttons.
Now we have spans which acts like a button to navigation between the pages as it has an onClick event handler in it .
The first span tag actually displays the left arrow and to add this feel that once ur in first page u dont need to see the left arrow we are using conditional rendering in class to make opacity go from 100 to 0
The second span tag is actually gonna display the numbers from 1 to 10 and helps us in navigating to different pages

```
[...Array(products.length / 10)]
```

the piece of code actually gets you the numbers ie from 1 to 10 in the format of an array so that we can map through it and display the span tag with each number in it
Isnt it a cool way of doing it !!!!
Now to add some premium feel to the button we are gonna use conditional rendering ie which ever page ur on its botton gonna have different then the rest

The last span tag is opposite of first span tag ie it displays the right arrow and once you reach the last page the right arrow is gonna disappear

Now we are gonna understand the onClick function logic that is attached to span tags :

```
 function selectPageHandler(selectedPage) {
    if (
      selectedPage >= 1 &&
      selectedPage <= products.length / 10 &&
      selectedPage != pages
    ) {
      setPages(selectedPage);
    }
  }
```

The above code is the one which navigates b/w different pages as it uses setPages variable which indeed the changes the pages variable from hook and navigates to required page as per user
Before setting it we have do some checking ie if selectedPage >=1 and selectedPage <= products.length / 10 && selectedPage != pages
the first condition is required cause we dont wanna use this when page = 0 and second one cause we want the pages to present within the range of products length and third one cause we want the selectedPage not equal to pages.

That's it about pagination that is better way of doing which is gonna be my next blog

Stay tuned
MaxDax
