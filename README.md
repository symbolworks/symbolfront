# Symbolfront

**A thin, context-driven client where applications expose only the actions that are meaningful in the current state.**

## The idea

This project explores whether input to a certain class of applications can be reduced to symbolic actions, contrasting the rich clients built on top of those applications. Considering the current state of an application, symbolic action set may be contextually discovered instead of memorizing the entire application instruction set.

The current action set, thus, depends on the application's current state while the current state changes after an action is entered to the client. The client remains as thin as possible while the application takes responsibility of receiving the next action and informing back the client of changed contextual display data and currently available actions.

## Example

The following example illustrates a typical Symbolfront session. The client contains no application-specific action vocabulary. Instead, during runtime, the current application context provides a set of actions available in that context.

When the example session starts, user interface shows:

```
-- context ---------------------------------------------------------------------

Applications.

    Welcome to applications directory. Please select an item.

  ▶ tasks ◀
    notes
    datatree
    microcalc

-- action ----------------------------------------------------------------------

 [ Choose ] [ Done ]
                                                               ▲        
                                                            ◀     ▶     
                                                               ▼        
```

We select `[ Choose ]`. UI responds:

```
-- context ---------------------------------------------------------------------

Tasks.
    
    Task List - The list is empty.

-- action ----------------------------------------------------------------------

 [ New-task ] [ Back ]
```

`[ New-task ]`

```
-- context ---------------------------------------------------------------------

Tasks. New task.

    Date:        ▶ <date> ◀
    Time:          <time>
    Description:   <string>

-- action ----------------------------------------------------------------------

 [ Cancel ]
 
 `   1   2   3   4   5   6   7   8   9   0   -   =     ⌫ 
 tab   q   w   e   r   t   y   u   i   o   p   [   ]   \ 
 caps   a   s   d   f   g   h   j   k   l   ;   '   enter      ▲ 
 shift    z   x   c   v   b   n   m   ,   .   /     shift   ◀     ▶ 
     alt                  space                  alt           ▼ 
```

`2026-09-08` `enter`

```
-- context ---------------------------------------------------------------------

Tasks. New task.

    Date:          2026-09-08
    Time:        ▶ <time> ◀
    Description:   <string>

-- action ----------------------------------------------------------------------

 [ Cancel ]
 
 `   1   2   3   4   5   6   7   8   9   0   -   =     ⌫ 
 tab   q   w   e   r   t   y   u   i   o   p   [   ]   \ 
 caps   a   s   d   f   g   h   j   k   l   ;   '   enter      ▲ 
 shift    z   x   c   v   b   n   m   ,   .   /     shift   ◀     ▶ 
     alt                  space                  alt           ▼ 
```

`10:00` `enter`

```
-- context ---------------------------------------------------------------------

Tasks. New task.

    Date:          2026-09-08
    Time:          10:00
    Description: ▶ <string> ◀

-- action ----------------------------------------------------------------------

 [ Accept ] [ Cancel ]

 `   1   2   3   4   5   6   7   8   9   0   -   =     ⌫ 
 tab   q   w   e   r   t   y   u   i   o   p   [   ]   \ 
 caps   a   s   d   f   g   h   j   k   l   ;   '   enter      ▲ 
 shift    z   x   c   v   b   n   m   ,   .   /     shift   ◀     ▶ 
     alt                  space                  alt           ▼ 
```

`Go to dentist` `[ Accept ]`

```
-- context ---------------------------------------------------------------------

Tasks.

    Task List - This week
    
     No.  Day, date, time                 Description

    ▶ 1.  Tue, 2026-09-08, 10:00          Go to dentist ◀

-- action ----------------------------------------------------------------------

 [ Filter ] [ Alter ] [ Done ]
                                                               ▲        
                                                            ◀     ▶     
                                                               ▼        

```

`[ Done ]`

```
-- context ---------------------------------------------------------------------

Applications.

    Welcome to applications directory. Please select an item.

  ▶ tasks ◀
    notes
    datatree
    microcalc

-- action ----------------------------------------------------------------------

 [ Choose ] [ Done ]
                                                               ▲        
                                                            ◀     ▶     
                                                               ▼        
```

`[ Done ]`

## Relation to backend

Symbolback is intended to serve as a backend service to Symbolfront. It is a symbolic virtual machine passing through various complex states, sending to Symbolfront only necessary contextual information. Thus, along the contextual display data, currently available actions are passed to Symbolfront as `A = α(S)` where `S` is the current application state and `α` is a function returning the available action set.

State transitions are defined as `S' = σ(S, a)` where `S` is the current state, the parameterized action `a` is an element of `A` action set, `σ` is a backend state transition function, and `S'` is the resulting state. That way, for proper functioning, client doesn't have to be aware of anything else but the current contextual display data and the current action set.

The upwards message communication from Symbolback to Symbolfront comes down to the following definition: `(D, A) = Context(S)`, where `D` is a current display data for state `S`, and `A` is a current action set for state `S`. In other words, Symbolback is the one responsible for guiding a user through application communication workflow without the client needing to be aware of application particular semantics.

```
• • • • • • • • • • • • • • • • • • • • • • • •
•                 Symbolfront                 •
• • • • • • • • • • • • • • • • • • • • • • • •
          ▲                         •
          •                         •
          •                         •
• • • • • • • • • • •     • • • • • • • • • • •
•   display data,   •     •       action,     •
•    action set     •     •     parameters    •
• • • • • • • • • • •     • • • • • • • • • • •
          •                         •
          •                         •
          •                         ▼
• • • • • • • • • • • • • • • • • • • • • • • •
•                 Symbolback                  •
• • • • • • • • • • • • • • • • • • • • • • • •

```

The client doesn't need to understand the transition function `σ`. It only needs to: display `D`, expose `A`, select/construct on its own a parameterized action `a ∈ A`, and send the `a` back.

## Summary

Symbolfront deliberately trades richer UI expressiveness for simplicity of appearance. The purpose of this project is to explore how far that trade-off can be taken. Richer UIs remain the better interface for many applications, but the question is how simple an interface can become while still supporting an interesting class of applications.
