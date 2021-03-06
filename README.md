# material-ui-popup-state

[![Build Status](https://travis-ci.org/jcoreio/material-ui-popup-state.svg?branch=master)](https://travis-ci.org/jcoreio/material-ui-popup-state)
[![Coverage Status](https://codecov.io/gh/jcoreio/material-ui-popup-state/branch/master/graph/badge.svg)](https://codecov.io/gh/jcoreio/material-ui-popup-state)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![npm version](https://badge.fury.io/js/material-ui-popup-state.svg)](https://badge.fury.io/js/material-ui-popup-state)

Takes care of the boilerplate for common Menu, Popover and Popper use cases.

Provides a [Custom React Hook](https://reactjs.org/docs/hooks-custom.html) that keeps track of the local state for a single popup, and functions to connect trigger, toggle, and
popover/menu/popper components to the state.

Also provides a [Render Props Component](https://reactjs.org/docs/render-props.html) that
keeps track of the local state for a single popup, and passes the state and
mutation functions to a child render function.

# Table of Contents

<!-- toc -->

- [Installation](#installation)
- [Examples with React Hooks](#examples-with-react-hooks)
  - [Menu](#menu)
  - [Popover](#popover)
  - [Popper](#popper)
- [React Hooks API](#react-hooks-api)
  - [Bind Functions](#bind-functions)
  - [`usePopupState`](#usepopupstate)
  - [`usePopupState` Props](#usepopupstate-props)
  - [`usePopupState` return value](#usepopupstate-return-value)
- [Examples with Render Props](#examples-with-render-props)
  - [Menu](#menu-1)
  - [Popover](#popover-1)
  - [Mouse Over Interaction](#mouse-over-interaction)
  - [Popper](#popper-1)
- [Render Props API](#render-props-api)
  - [Bind Functions](#bind-functions-1)
  - [`PopupState` Props](#popupstate-props)

<!-- tocstop -->

# Installation

```sh
npm install --save material-ui-popup-state
```

# Examples with React Hooks

## Menu

```js
import * as React from 'react'
import Button from '@material-ui/core/Button'
import Menu from '@material-ui/core/Menu'
import MenuItem from '@material-ui/core/MenuItem'
import { usePopupState, bindTrigger, bindMenu } from 'material-ui-popup-state/hooks'

const MenuPopupState = () => {
  const popupState = usePopupState({variant: 'popover', popupId: 'demoMenu'})
  return (
    <div>
      <Button variant="contained" {...bindTrigger(popupState)}>
        Open Menu
      </Button>
      <Menu {...bindMenu(popupState)}>
        <MenuItem onClick={popupState.close}>Cake</MenuItem>
        <MenuItem onClick={popupState.close}>Death</MenuItem>
      </Menu>
    </div>
  )
)

export default MenuPopupState
```

## Popover

```js
import React from 'react'
import PropTypes from 'prop-types'
import { withStyles } from '@material-ui/core/styles'
import Typography from '@material-ui/core/Typography'
import Button from '@material-ui/core/Button'
import Popover from '@material-ui/core/Popover'
import {
  usePopupState,
  bindTrigger,
  bindPopover,
} from 'material-ui-popup-state/hooks'

const styles = theme => ({
  typography: {
    margin: theme.spacing.unit * 2,
  },
})

const PopoverPopupState = ({ classes }) => {
  const popupState = usePopupState({
    variant: 'popover',
    popupId: 'demoPopover',
  })
  return (
    <div>
      <Button variant="contained" {...bindTrigger(popupState)}>
        Open Popover
      </Button>
      <Popover
        {...bindPopover(popupState)}
        anchorOrigin={{
          vertical: 'bottom',
          horizontal: 'center',
        }}
        transformOrigin={{
          vertical: 'top',
          horizontal: 'center',
        }}
      >
        <Typography className={classes.typography}>
          The content of the Popover.
        </Typography>
      </Popover>
    </div>
  )
}

PopoverPopupState.propTypes = {
  classes: PropTypes.object.isRequired,
}

export default withStyles(styles)(PopoverPopupState)
```

## Popper

```js
import React from 'react'
import PropTypes from 'prop-types'
import { withStyles } from '@material-ui/core/styles'
import Typography from '@material-ui/core/Typography'
import Button from '@material-ui/core/Button'
import Popper from '@material-ui/core/Popper'
import {
  usePopupState,
  bindToggle,
  bindPopper,
} from 'material-ui-popup-state/hooks'
import Fade from '@material-ui/core/Fade'
import Paper from '@material-ui/core/Paper'

const styles = theme => ({
  typography: {
    padding: theme.spacing.unit * 2,
  },
})

const PopperPopupState = ({ classes }) => {
  const popupState = usePopupState({ variant: 'popper', popupId: 'demoPopper' })
  return (
    <div>
      <Button variant="contained" {...bindToggle(popupState)}>
        Toggle Popper
      </Button>
      <Popper {...bindPopper(popupState)} transition>
        {({ TransitionProps }) => (
          <Fade {...TransitionProps} timeout={350}>
            <Paper>
              <Typography className={classes.typography}>
                The content of the Popper.
              </Typography>
            </Paper>
          </Fade>
        )}
      </Popper>
    </div>
  )
}

PopperPopupState.propTypes = {
  classes: PropTypes.object.isRequired,
}

export default withStyles(styles)(PopperPopupState)
```

# React Hooks API

## Bind Functions

`material-ui-popup-state/hooks` exports several helper functions you can use to
connect components easily:

- `bindMenu`: creates props to control a `Menu` component.
- `bindPopover`: creates props to control a `Popover` component.
- `bindPopper`: creates props to control a `Popper` component.
- `bindTrigger`: creates props for a component that opens the popup when clicked.
- `bindToggle`: creates props for a component that toggles the popup when clicked.

To use one of these functions, you should call it with the object
returned by `usePopupState` and spread the return value into the desired
element:

```js
import * as React from 'react'
import Button from '@material-ui/core/Button'
import Menu from '@material-ui/core/Menu'
import MenuItem from '@material-ui/core/MenuItem'
import {
  usePopupState,
  bindTrigger,
  bindMenu,
} from 'material-ui-popup-state/hooks'

const MenuPopupState = () => {
  const popupState = usePopupState({ variant: 'popover', popupId: 'demoMenu' })
  return (
    <div>
      <Button variant="contained" {...bindTrigger(popupState)}>
        Open Menu
      </Button>
      <Menu {...bindMenu(popupState)}>
        <MenuItem onClick={popupState.close}>Cake</MenuItem>
        <MenuItem onClick={popupState.close}>Death</MenuItem>
      </Menu>
    </div>
  )
}

export default MenuPopupState
```

## `usePopupState`

This is a [Custom Hook](https://reactjs.org/docs/hooks-custom.html) that uses `useState` internally, therefore the [Rules of Hooks](https://reactjs.org/docs/hooks-rules.html) apply to `usePopupState`.

## `usePopupState` Props

### `variant` (`'popover'` or `'popper'`, **required**)

Use `'popover'` if your popup is a `Popover` or `Menu`; use `'popper'` if your
popup is a `Popper`.

Right now this only affects whether `bindTrigger`/`bindToggle`/`bindHover` return
an `aria-owns` prop or an `aria-describedby` prop.

### `popupId` (`string`, **optional** but strongly encouraged)

The `id` for the popup component. It will be passed to the child props so that
the trigger component may declare the same id in an ARIA prop.

## `usePopupState` return value

An object with the following properties:

- `open(eventOrAnchorEl)`: opens the popup
- `close()`: closes the popup
- `toggle(eventOrAnchorEl)`: opens the popup if it is closed, or closes the popup if it is open.
- `setOpen(open, [eventOrAnchorEl])`: sets whether the popup is open. `eventOrAnchorEl` is required if `open` is truthy.
- `isOpen`: `true`/`false` if the popup is open/closed
- `anchorEl`: the current anchor element (`null` when the popup is closed)
- `popupId`: the `popupId` prop you passed to `PopupState`
- `variant`: the `variant` prop you passed to `PopupState`

# Examples with Render Props

## Menu

```js
import * as React from 'react'
import Button from '@material-ui/core/Button'
import Menu from '@material-ui/core/Menu'
import MenuItem from '@material-ui/core/MenuItem'
import PopupState, { bindTrigger, bindMenu } from 'material-ui-popup-state'

const MenuPopupState = () => (
  <PopupState variant="popover" popupId="demoMenu">
    {popupState => (
      <React.Fragment>
        <Button variant="contained" {...bindTrigger(popupState)}>
          Open Menu
        </Button>
        <Menu {...bindMenu(popupState)}>
          <MenuItem onClick={popupState.close}>Cake</MenuItem>
          <MenuItem onClick={popupState.close}>Death</MenuItem>
        </Menu>
      </React.Fragment>
    )}
    }
  </PopupState>
)

export default MenuPopupState
```

## Popover

```js
import React from 'react'
import PropTypes from 'prop-types'
import { withStyles } from '@material-ui/core/styles'
import Typography from '@material-ui/core/Typography'
import Button from '@material-ui/core/Button'
import Popover from '@material-ui/core/Popover'
import PopupState, { bindTrigger, bindPopover } from 'material-ui-popup-state'

const styles = theme => ({
  typography: {
    margin: theme.spacing.unit * 2,
  },
})

const PopoverPopupState = ({ classes }) => (
  <PopupState variant="popover" popupId="demoPopover">
    {popupState => (
      <div>
        <Button variant="contained" {...bindTrigger(popupState)}>
          Open Popover
        </Button>
        <Popover
          {...bindPopover(popupState)}
          anchorOrigin={{
            vertical: 'bottom',
            horizontal: 'center',
          }}
          transformOrigin={{
            vertical: 'top',
            horizontal: 'center',
          }}
        >
          <Typography className={classes.typography}>
            The content of the Popover.
          </Typography>
        </Popover>
      </div>
    )}
  </PopupState>
)

PopoverPopupState.propTypes = {
  classes: PropTypes.object.isRequired,
}

export default withStyles(styles)(PopoverPopupState)
```

## Mouse Over Interaction

```js
import React from 'react'
import PropTypes from 'prop-types'
import { withStyles } from '@material-ui/core/styles'
import Typography from '@material-ui/core/Typography'
import Popover from '@material-ui/core/Popover'
import PopupState, { bindHover, bindPopover } from 'material-ui-popup-state'

const styles = theme => ({
  popover: {
    pointerEvents: 'none',
  },
  paper: {
    padding: theme.spacing.unit,
  },
})

const HoverPopoverPopupState = ({ classes }) => (
  <PopupState variant="popover" popupId="demoPopover">
    {popupState => (
      <div>
        <Typography {...bindHover(popupState)}>
          Hover with a Popover.
        </Typography>
        <Popover
          {...bindPopover(popupState)}
          className={classes.popover}
          classes={{
            paper: classes.paper,
          }}
          anchorOrigin={{
            vertical: 'bottom',
            horizontal: 'center',
          }}
          transformOrigin={{
            vertical: 'top',
            horizontal: 'center',
          }}
          disableRestoreFocus
        >
          <Typography>The content of the Popover.</Typography>
        </Popover>
      </div>
    )}
  </PopupState>
)

HoverPopoverPopupState.propTypes = {
  classes: PropTypes.object.isRequired,
}

export default withStyles(styles)(HoverPopoverPopupState)
```

## Popper

```js
import React from 'react'
import PropTypes from 'prop-types'
import { withStyles } from '@material-ui/core/styles'
import Typography from '@material-ui/core/Typography'
import Button from '@material-ui/core/Button'
import Popper from '@material-ui/core/Popper'
import PopupState, { bindToggle, bindPopper } from 'material-ui-popup-state'
import Fade from '@material-ui/core/Fade'
import Paper from '@material-ui/core/Paper'

const styles = theme => ({
  typography: {
    padding: theme.spacing.unit * 2,
  },
})

const PopperPopupState = ({ classes }) => (
  <PopupState variant="popper" popupId="demoPopper">
    {popupState => (
      <div>
        <Button variant="contained" {...bindToggle(popupState)}>
          Toggle Popper
        </Button>
        <Popper {...bindPopper(popupState)} transition>
          {({ TransitionProps }) => (
            <Fade {...TransitionProps} timeout={350}>
              <Paper>
                <Typography className={classes.typography}>
                  The content of the Popper.
                </Typography>
              </Paper>
            </Fade>
          )}
        </Popper>
      </div>
    )}
  </PopupState>
)

PopperPopupState.propTypes = {
  classes: PropTypes.object.isRequired,
}

export default withStyles(styles)(PopperPopupState)
```

# Render Props API

## Bind Functions

`material-ui-popup-state` exports several helper functions you can use to
connect components easily:

- `bindMenu`: creates props to control a `Menu` component.
- `bindPopover`: creates props to control a `Popover` component.
- `bindPopper`: creates props to control a `Popper` component.
- `bindTrigger`: creates props for a component that opens the popup when clicked.
- `bindToggle`: creates props for a component that toggles the popup when clicked.
- `bindHover`: creates props for a component that opens the popup while hovered.

To use one of these functions, you should call it with the props `PopupState`
passed to your child function, and spread the return value into the desired
element:

```js
import * as React from 'react'
import Button from '@material-ui/core/Button'
import Menu from '@material-ui/core/Menu'
import MenuItem from '@material-ui/core/MenuItem'
import PopupState, { bindTrigger, bindMenu } from 'material-ui-popup-state'

const MenuPopupState = () => (
  <PopupState variant="popover" popupId="demoMenu">
    {popupState => (
      <React.Fragment>
        <Button variant="contained" {...bindTrigger(popupState)}>
          Open Menu
        </Button>
        <Menu {...bindMenu(popupState)}>
          <MenuItem onClick={popupState.close}>Cake</MenuItem>
          <MenuItem onClick={popupState.close}>Death</MenuItem>
        </Menu>
      </React.Fragment>
    )}
  </PopupState>
)

export default MenuPopupState
```

## `PopupState` Props

### `variant` (`'popover'` or `'popper'`, **required**)

Use `'popover'` if your popup is a `Popover` or `Menu`; use `'popper'` if your
popup is a `Popper`.

Right now this only affects whether `bindTrigger`/`bindToggle`/`bindHover` return
an `aria-owns` prop or an `aria-describedby` prop.

### `popupId` (`string`, **optional** but strongly encouraged)

The `id` for the popup component. It will be passed to the child props so that
the trigger component may declare the same id in an ARIA prop.

### `children` (`(popupState: InjectedProps) => ?React.Node`, **required**)

The render function. It will be called with an object containing the following
props (exported as the `InjectedProps` type):

- `open(eventOrAnchorEl)`: opens the popup
- `close()`: closes the popup
- `toggle(eventOrAnchorEl)`: opens the popup if it is closed, or closes the popup if it is open.
- `setOpen(open, [eventOrAnchorEl])`: sets whether the popup is open. `eventOrAnchorEl` is required if `open` is truthy.
- `isOpen`: `true`/`false` if the popup is open/closed
- `anchorEl`: the current anchor element (`null` when the popup is closed)
- `popupId`: the `popupId` prop you passed to `PopupState`
- `variant`: the `variant` prop you passed to `PopupState`
