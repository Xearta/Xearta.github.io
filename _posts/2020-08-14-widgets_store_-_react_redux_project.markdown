---
layout: post
title:      "Widgets Store - React Redux Project"
date:       2020-08-14 14:08:31 +0000
permalink:  widgets_store_-_react_redux_project
---

## React - Redux Modules
The React module went smoothly for me and I really enjoy using React. I REALLY enjoy **JSX** and how simple it is to create HTML elements with JavaScript. Using `props` feels very natural and just makes sense. 

However, Redux on the other hand, *oh boy...* I hadn't **really** struggled on any of the modules until Redux. Redux was a *doozy*  for me. Conceptionally, it makes sense to have a `store` to house all of the `reducers` which are updated by the `actions` but, in practice, *no*. After I finished the Redux module, I had to do **a lot** of extra research to better understand how Redux actually works in practice.

## The Inspiration
After doing *a lot* of extra research on Redux, I had a good enough understand on how it worked to be able to create a application using React and Redux. *At least, I thought...* I decided to create the **Widgets Store** because I wanted to broaden my portfolio and create an E-commerce web site. I figured an 'Amazon' look-a-like store would be pretty easy but a bit challenging to implement some bonus features. 

## Starting The Project
The first step in this project was to create a template design and a backend. I created a template design using HTML and CSS for a homepage that I was happy with. Then, I created my backend using:

`rails new react-ecommerce-backend --api --database=postgresql`

I used Postgres as a database so that I can host my application on Heroku.


## Users
One of the challenging features of my application was to deal with Users. After creating the `products` model, I generated a `users` model to handle User authentication with `bcrypt` and session information with a `sessions_controller`.  The Sessions Controller works with the Application Controller helper methods to check if a `session` exists and matches any of the `users` in the database. If so, it will automatically log the user in.

I also created redirect methods in React to handle if I use tries to go to a URL that they aren't supposed to be able. 

For Example: If a user tries to go to: `http://localhost:3000/placeorder` they will automatically be redirected to the folllowing urls depending on the information that is missing.

```
if (!shipping.address) {
    props.history.push('/shipping');
} else if (!payment.paymentMethod) {
    props.history.push('/payment');
}
```

## Checkout
The checkout functionality was one of the hardest parts to incorperate. Let's break it down in steps:

1. Make the 'Proceed To Checkout Button' work if logged in else, redirect to sign in.
```
<button onClick={checkoutHandler}>Proceed To Checkout</button>
```
```
const checkoutHandler = () => {
  props.history.push('/signin?redirect=shipping');
};
```

2. Input shipping information and retain information through pagination
```
const submitHandler = event => {
    event.preventDefault();
    dispatch(saveShipping({ address, city, postalCode, country }));
    props.history.push('/payment');
};
```
*cartAction.js*
```
export const saveShipping = data => dispatch => {
    dispatch({ type: 'CART_SAVE_SHIPPING', payload: data });
};
```
*cartReducers.js*
```
case 'CART_SAVE_SHIPPING':
    return { ...state, shipping: action.payload };
```

3. Select a payment method and retain information through pagination
```
const submitHandler = event => {
    event.preventDefault();
    dispatch(savePayment({ paymentMethod }));
    props.history.push('/placeorder');
};
```
*The action and reducer functions are identical to the shipping*

4. Review order and make the 'Place Order' button work
```
const placeOrderHandler = () => {
    dispatch(createOrder({ user_id, cartItems, shipping, payment, itemsPrice, shippingPrice, taxPrice, totalPrice }));
};
useEffect(() => {
    if (success) {
      props.history.push(`/orders/${order.id}`);
      orderCreate.success = false;
      cart.cartItems = [];
    }
  }, [success, orderCreate.success, props.history]);
```
Dispatch the `createOrder` action with the order object and update the page with `useEffect()` if the order creation is sucessful.

5. View Order Summary in the *orderContainer.js*


## Future Updates
I will continue to work on this project and improve it beyond what it is currently. I would love to add more products, better filtering, more filtering options, more payment methods, actual payment authentication, and most importantly, better styling and user experience!
