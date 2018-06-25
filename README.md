action-reduxers: a wrapper around `redux-action-tools` for obscesive people
===

I've used `redux-action-tools` quite a bit and found myself reimplementing
this layer in all my projects, so here's an APIsh view

```js
// some-reducer.js

import {} from 'action-reduxers'
import myAPI from './api'

const creators = {
    add: {
        promiseCreator: (config) => api.fetch(config), // returns a Promise
        handler: ({stuff, ...state}, {payload}) => ({
            ...state,
            stuff: Object.assign({[payload.id]: payload}, stuff)
        })
    },
    remove: {
        promiseCreator: (instance) => Promise.resolve(instance.destroy()),
        handler: ({stuff, ...state}, {payload}) => {
            delete stuff[payload.id]
            return {
                ...state,
                stuff
            }
        }
    }
}

const makeActionTypes = (creators) => {
    const actionKeys = Object.keys(creators)

    return actionKeys.reduce((actionTypes, type) => (
        Object.assign(actionTypes, {
            [type]: `TG/PHONEAPP/${type}`
        })
    ), {})
}
const actionTypes = makeActionTypes(creators)

export default {
    actions: utils.makeActions(actionTypes, creators),
    reducer: utils.makeReducer(actionTypes, creators, 
        {
            items: [
                { id: 0, phone: "+54 911 1234 5678", name: "Mercedes Sosa"},
                { id: 1, phone: "+54 911 8765 4321", name: "Carlos Gardel"},
                { id: 2, phone: "+55 11 8765 4321", name: "Caetano Veloso"},
            ]
        }
    )
}
```
