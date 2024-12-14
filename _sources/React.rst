=====
React
=====

1. `What is React?`_
2. `Folder structure`_
3. `Components`_
4. `Props & State`_
5. `Hooks`_
6. `Styled components`_

What is React?
==============

* JS library for building UI
* components are reusable

`back to top <#top>`_

Folder structure
================

* public
    * will be built by dev environment
    * finished file used for running the app
* src
    * main component folder

`back to top <#top>`_

Components
==========

* uses **JSX** syntax
    * JavaScript, XML
    * looks like HTML but closer to JS and uses camelCase property naming convention
    * prevents injection attacks
* class component & functional component
* can only return one component, need to wrap if want to return multiple
* always name components with capital first letter
* ``<></>``: fragment element, use to wrap around component to return without naming it

`back to top <#top>`_

Props & State
=============


state
-----
    * embed the state in parent component to be able to use in multiple child components
    * states should be considered immutable
    * always use state setter, not modify directly as it will not re-render

        .. code-block:: js

           const [getState, setState] = useState(initialValue)
           const handleState = () => setState(prev => !prev)


    * functional components can have many states, unlike class components which can only have one

props
-----
    * an object that can be created and will get send along into component
    * can create many properties on an object

    .. code-block:: react

       return(
       <Component1 prop1={getState} prop2='myState' />
       )


    * main difference from state is that props are passed in components and never change props
    in the component that receives them
    * when the prop value changes, the component will re-render with new value
    * props make component dynamic
    * **children**
        - default prop to use when nesting stuff inside a component
* PropTypes
    * to check propTypes when not using TypeScript

    .. code-block:: js

       import PropTypes from 'prop-types'
       Component.propTypes = {
       prop: PropTypes.object
       }


`back to top <#top>`_

Hooks
=====

* useState
* useEffect
    * always trigger on initial render
    * for side effects, usually to grab data

    .. code-block:: js

       useEffect(() => {}, [])
       // empty array, will only run once


    * returning a function will be triggered before each new render

    .. code-block:: js

       useEffect(() => {return FUNCTION}, [])


* useContext
    * used to hold global state
* useReducer
    * can be used instead of useState for more complex state
* useCallback
    * to memorize stuff without recreating
    * wrap around regular function not to create an infinity loop
* useMemo
    * to memorize stuff without recreating
* useRef
    * to create mutable value that will not trigger a re-render
* useImperativeHandle
* useLayoutEffect
    * similar to useEffect, differ only when triggered
* useDebugValue
* custom hooks
    * name always start with 'use' (eg. useMyHook)

`back to top <#top>`_

Styled components
=================

* can have scoped CSS, same class names for different components
* can use syntax, like Sass, and nest stuff
* can have props inside

.. code-block:: js

   import styled from 'styled-components'
   
   const Button = styled.button`
     :root {
       --maxWidth: 1280px;
     }
   `


`back to top <#top>`_
