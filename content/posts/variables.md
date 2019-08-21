---
title: "Variables"
description: "Variables and Clean Code with JS."
date: 2019-08-12
order: 2
lang: "en"
---

## **Variables**

### Use meaningful and pronounceable variable names

**Bad:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Good:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

### Use the same vocabulary for the same type of variable

**Bad:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good:**

```javascript
getUser();
```

### Use searchable names

We will read more code than we will ever write. It's important that the code we
do write is readable and searchable. By _not_ naming variables that end up
being meaningful for understanding our program, we hurt our readers.
Make your names searchable. Tools like
[buddy.js](https://github.com/danielstjules/buddy.js) and
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
can help identify unnamed constants.

**Bad:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**Good:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

### Use explanatory variables

**Bad:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Good:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

### Avoid Mental Mapping

Explicit is better than implicit.

**Bad:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**Good:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

### Don't add unneeded context

If your class/object name tells you something, don't repeat that in your
variable name.

**Bad:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car) {
  car.carColor = "Red";
}
```

**Good:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car) {
  car.color = "Red";
}
```

### Use default arguments instead of short circuiting or conditionals

Default arguments are often cleaner than short circuiting. Be aware that if you
use them, your function will only provide default values for `undefined`
arguments. Other "falsy" values such as `''`, `""`, `false`, `null`, `0`, and
`NaN`, will not be replaced by a default value.

**Bad:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Good:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```
