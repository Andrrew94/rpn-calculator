# rpn-calculator

### About the app
This is a RPN (Reverse polish notation) calculator implemented using **Vue.js** <br>

The reason behind choosing Vue.js as a frameowrk to implement this is because in my opinion Vue is one of the most powerful and easy to use javascript framworks. It's really easy to setup and get started with any project, its features providing a fast clean implementation.

The project structure is simple, including a Layout which renders a Header and the Calculator itself. <br>
The styling was done using **SCSS** and **BEM** methodology, using them together provides a really clean nested css.

The calculator is able to execute the 4 basic operators in two ways, multiple line inputs and single line inputs. <br>

## Multiple Inputs:
```
> 5 
5
> 8
8
> +
13
```

## Single Line Input:
```
> 5 5 5 8 + + -
-13
> 13 +
0
```

## Negatives and Floats
```
> -3
-3.0
> -2
-2.0
> *
6.0
> 5
5.0
> +
11.0
```

## Floats and Division
```
> 5
5
> 9
9
> 1
1
> -
8
> /
0.625
```

**Some of the things I would have done differently given I had the time**
  * Use scss mixins / includes / functions / variables for future use in different components
  * Break the calculator into multiple components if some of the pieces turn out to be needed somewhere else
  * Better validation and error handling, use a more complex regex for the user input


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```
