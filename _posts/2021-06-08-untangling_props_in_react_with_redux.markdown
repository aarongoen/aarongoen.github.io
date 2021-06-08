---
layout: post
title:      "Untangling Props in React with Redux"
date:       2021-06-08 11:20:43 -0400
permalink:  untangling_props_in_react_with_redux
---


Trying to organize and keep track of state in React can get tricky, especially if you're passing down props to different children and grandchildren. One way to make this process easier is to use Redux to keep track of state globally.

I built an app which helps church music directors plan music according to liturgical day. So, for example, if the user would like to discover new choral music appropriate to the bible readings for the Sunday before Christmas (also known as Advent 4), they could find suggested pieces which quote or reference those bible readings for that day. The user can add new pieces to that list as well. Here's a simplified version of the schema: ![](https://drive.google.com/file/d/1zag8cy1PJk1ScqAS-9kGdSPkCsZ0U1RZ/view?usp=sharing)


With my components structured like so: [](https://drive.google.com/file/d/1EQRFx8iEu1pnJI2bk7BFta4HEivMLYiB/view?usp=sharing)

You can see that the pieces inherit from the DayShow component. It's possible that DayShow could inherit from DaysList but in order to expand the capabilities of the app to include a PieceEdit component for example, it would be much easier to have the state live in a separate location. This would be the store.

The store is a separate file which stores the state. It works with Redux actions and reducers. The actions are processes such as CreatePiece. The reducers are processes which change the state after the actions are completed. So, in the CreatePiece example, the user fills out and submits the form. The form sends the data via a POST request to the backend by calling the createPiece function. If it's successfully completed on the backend, the dayReducer updates the state on front end to reflect that change. Simple concept but a bit more complex in execution. 

Firstly, I had to create the store. I created a redux folder and a store.js file in it . Then I imported the code for it like so: `import {createStore} from 'redux';` In my index.js file, I `import store from './redux/store';`. Then I wrapped my `<App />` in the store to give all components access to the store:

```
ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

I stored my createPiece function in another folder in my Redux folder called `actions`. It does a typical fetch post request but in the last `.then` statement `dispatches` an action called `ADD_PIECE` to the dayReducer. This is the part I was most confused about. In tutorials, the process of dispatching the reducer is triggered after a process is completed. In this case, if and when the post fetch request is successful, then update the state using `ADD_PIECE` from the dayReducer. This bit looks like: 

```
export const createPiece = (piece, day_id) => {
        return (dispatch) => {
            fetch(`http://localhost:3000/days/${day_id}/pieces`, {
                headers: {
                    'Accept': 'application/json',
                    'Content-Type': 'application/json'
                },
                method: 'POST',
            body: JSON.stringify(piece)
            })
            .then((res) => {
                console.log(res)
                if (res.ok) {
                    return res.json();
                } else {
                    throw new Error(res.statusText);
                }
            })
            .then(data => {
                console.log(data)
                dispatch({type: 'ADD_PIECE', payload: data})
            })
            .catch((err) => console.log(err))
        }
    }
```
		
So this function called `createPiece` in the pieceActions.js file in the actions folder (which lives in the redux folder) takes the new piece instance from the PieceForm, and its corresponding day_id, and makes a POST fetch request to the backend. If it's successfully instantiated in the backend, then that same data is dispatched to an `ADD_PIECE` function in the dayReducer.

The dayReducer takes care of updating the state to reflect what happened in the backend. Reducers need to have an initial state be declared. I did that like so: `const reduceDay = (state = {days: []}, action) => {}`. This function declares state as an empty array called 'days'. It also takes in the action `ADD_PIECE`. Since I'm going to want this function to respond to different actions like fetching the days and showing a day, I used a `switch` statement: 

```
const reduceDay = (state = {days: []}, action) => {
    switch(action.type) {
        case "FETCH_DAYS_SUCCESS":
            return action.payload
        case "ADD_PIECE": {
                let oldDay = state.find(day => day.id === action.payload.day.day_id)
                let filteredDays = state.filter(day => day !== oldDay)
                let newPieceArray = oldDay.pieces.concat(action.payload)
                oldDay.pieces = newPieceArray
            return {...state, days: [...filteredDays, oldDay.pieces]}
            }
        case "SHOW_DAY_SUCCESS":
            return action.payload
        default:
            return state;
    }
}

export default reduceDay;
```
With the switch statement, you can have the dayReducer respond to your typical CRUD actions. The action.type in the switch statement refers to the `type: 'ADD_PIECE` in the `dispatch({type: 'ADD_PIECE', payload: data})` function. 

I found the `ADD_PIECE` action particularly challenging. Originally, I thought I would search for the day in question and insert a new piece in the pieces array. It turns out that, after some help from a colleague (Thanks again, Becca!), I needed to find the day, create a new array of days minus the day in question, concatenate the new piece onto the pieces array in the day in question, i.e. `oldDay`, then set this `newPieceArray` to the piece array of `oldDay`. Then I had to return the state using the spread operator (so as not to directly set the state but rather modify a copy of state) and tack on the modified day, i.e. `oldDay.pieces` onto the `filteredDays`.




		
		

