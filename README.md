# Symbolfront

**A context-driven frontend that exposes only the context and dependent actions originating from the middleware's current state.**

## The idea

This project explores whether a frontend can be separated from middleware, exposing in frontend only the current context and available action set that may operate on that context. The middleware becomes responsible for maintaining application states which respond to sent actions, and which generate a new context.

From the user side, the frontend still serves to output context data and input contextually dependent actions, but its role is more limited than in classical frontends that embrace the entire user interfacing logic. In Symbolfront, a considerable part of that logic is delegated to the middleware which maintains its states synchronized with the bare frontend.

The frontend thus becomes a very thin client that manages raw input and output from the user. The middleware possibly offers an arbitrary set of controls that output the current context. Some minimal set of controls may include, but is not limited to messages, lists, tables and input forms. Responding back with an action, the user decides which one to send to the middleware, expecting a new context to appear in the frontend.

## Example

The following example illustrates a typical Symbolfront session. The frontend contains no application-specific action vocabulary. Instead, during runtime, as the context changes, the middleware provides a set of actions available in that context.

When the example session starts, user interface shows:

```
[ Tasks ]

-- context ---------------------------------------------------------------------
    
  Task List - The list is empty

-- actions ---------------------------------------------------------------------

  [ New task ]  [ Done ]

--------------------------------------------------------------------------------
```

We click `[ New task ]`. The UI responds:

```
[ Tasks ]

-- context ---------------------------------------------------------------------

  New task

  |        Date: ▶<YYYY-MM-DD>◀ |
  |        Time:  <HH:MM>       |
  | Description:  <text>        |

-- actions ---------------------------------------------------------------------

  [ Cancel ]
 
-- cursor -- alpha -- num -- mouse -- touch ------------------------------------
```

We type `2026-09-08` and press `enter`.

```
[ Tasks ]

-- context ---------------------------------------------------------------------

  New task

  |        Date:  2026-09-08 |
  |        Time: ▶<HH:MM>◀   |
  | Description:  <text>     |

-- actions ---------------------------------------------------------------------

  [ Cancel ]
 
-- cursor -- alpha -- num -- mouse -- touch ------------------------------------
```

We type `10:00` and press `enter`.

```
[ Tasks ]

-- context ---------------------------------------------------------------------

  New task

  |        Date:  2026-09-08 |
  |        Time:  10:00      |
  | Description: ▶<text>◀    |

-- actions ---------------------------------------------------------------------

  [ Cancel ]

-- cursor -- alpha -- num -- mouse -- touch ------------------------------------
```

We type `Go to dentist` and press `enter`. New task is added.

```
[ Tasks ]

-- context ---------------------------------------------------------------------

  Task List - This week

  |  No.  Day, date, time           Description   |
  |                                               |
  | ▶ 1.◀ Tue, 2026-09-08, 10:00    Go to dentist |

-- actions ---------------------------------------------------------------------

  [ Filter ]  [ Alter list ]  [ Done ]
 
-- cursor -- mouse -- touch ----------------------------------------------------
```

We click `[ Done ]`. The session ends.

## Relation between frontend and middleware

Middleware is a symbolic virtual machine passing through various complex states, sending to frontend only necessary contextual information. Thus, along the contextual display data, currently available actions are passed to the frontend as `A = α(S)` where `S` is the current application state and `α` is a function returning the available action set.

State transitions are defined as `S' = σ(S, a)` where `S` is the current state, action `a` is an element of `A` action set, `σ` is a middleware state transition function, and `S'` is the resulting state. That way, for proper functioning, the frontend doesn't have to be aware of anything else but the current contextual display data and the current action set.

The upwards message communication from middleware to frontend brings the following definition: `(C', A') = Interface(S')`, where `C'` is a current context for state `S'`, and `A'` is a current action set for state `S'`. In other words, middleware is the one responsible for guiding a user through application communication workflow without the frontend needing to be aware of application particular semantics.

```
• • • • • • • • • • • • • • • • • • • • • • • •
•                  frontend                   •
• • • • • • • • • • • • • • • • • • • • • • • •
          ▲                         •
          •                         •
          •                         •
• • • • • • • • • • •     • • • • • • • • • • •
•     context,      •     •       action      •
•    action set     •     •                   •
• • • • • • • • • • •     • • • • • • • • • • •
          •                         •
          •                         •
          •                         ▼
• • • • • • • • • • • • • • • • • • • • • • • •
•                  middleware                 •
• • • • • • • • • • • • • • • • • • • • • • • •
```

The frontend doesn't need to understand the transition function `σ`. It only needs to: display `C`, expose `A`, select an action `a ∈ A`, and send the `a` back.

## Summary

Frontends commonly manage the entire user interfacing logic. Symbolfront separates that role into (1) bare input/output user frontend and (2) computational middleware that communicates with the minimal frontend. As a consequence, user interface offers a particular kind of interaction with the user, basing its essence in two clearly differentiated notions: action and response. Using the frontend, contextually dependent actions sent by users land at the middleware which responds back to the frontend, sending a new context and assigned set of newly available actions.

The separation of user actions from computing responses may hopefully bring some benefits that may include consistency and simplicity in designing and experiencing application user interface. Being an experimental excursion, this architecture still needs to prove itself in real use cases.
