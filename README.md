# Symbolfront

**A thin, context-driven client where applications expose only the commands that are meaningful in the current state.**

## The idea

This project explores whether input to a certain class of applications can be reduced to symbolic commands, contrasting the rich clients built on top of those applications. Considering the current state of an application, symbolic command set may be contextually discovered instead of memorizing the entire application instruction set.

The current command set, thus, depends on the application's current state while the current state changes after a command is entered to the client. The client remains as thin as possible while the application takes responsibility of receiving the next command and informing the client of changed contextual display data and currently available commands.

## Example

The following example illustrates a typical Symbolfront session. The client contains no application-specific command vocabulary. Instead, during runtime, the current application context provides a set of commands available in that context.

When the example session starts, user interface shows:

```
Applications.

    Welcome to applications directory. Please enter a command.

Commands.

    email
    tasks
    notes
    calc

    done

>
```

We write `tasks` and press [enter]. UI responds:

```
Tasks.
    
    Task List - The list is empty.

Commands.

    new-task
    
    done

>
```

We enter `new-task`. UI responds:

```
Tasks. New task.

    Task

     No.  Field             Data
    -------------------------------------------------------
    ▶ 1.  Date and time     <date-time>
      2.  Description       <string>
      3.  OK?
    -------------------------------------------------------

Commands.

    <No.>

    <date-time>
    reject

>
```

We enter `"2026-09-08, 10:00"`. UI responds:

```
Tasks. New task.

    Task

     No.  Field             Data
    -------------------------------------------------------
      1.  Date and time     "2026-09-08, 10:00"
    ▶ 2.  Description       <string>
      3.  OK?
    -------------------------------------------------------

Commands.

    <No.>

    <string>
    reject

>
```

We enter `"Go to dentist"`. UI responds:

```
Tasks. New task.

    Task

     No.  Field             Data
    -------------------------------------------------------
      1.  Date and time     "2026-09-08, 10:00"
      2.  Description       "Go to dentist"
    ▶ 3.  OK?
    -------------------------------------------------------

Commands.

    <No.>

    accept
    reject

>
```

We enter `accept`. We're back to the tasks list:

```
Tasks.

    Task List - This week
    
      No.  Day, date, time                 Description
    -------------------------------------------------------------------------
    ▶  1.  Tue, 2026-09-08, 10:00          Go to dentist
    -------------------------------------------------------------------------

Commands.

    <No.>
    filter
    alter

    done

>
```

We enter `done`. We're back to the applications directory:

```
Applications.

    Welcome to applications directory. Please enter a command.

Commands.

    email
    tasks
    notes
    calc

    done

>
```

We enter `done`. The session ends.

## Relation to backend

Symbolback is intended to serve as a backend service to Symbolfront. It is a symbolic virtual machine passing through various complex states, sending to Symbolfront only necessary contextual information. Thus, along the contextual display data, currently available commands are passed to Symbolfront as `K = κ(S)` where `S` is the current application state and `κ` is a function returning the available command set.

State transitions are defined as `S' = σ(S, c)` where `S` is the current state, the command `c` is an element of `K` command set, `σ` is a backend state transition function, and `S'` is the resulting state. That way, for proper functioning, client doesn't have to be aware of anything else but the current contextual display data and the current command set.

The upwards message communication from Symbolback to Symbolfront comes down to the following definition: `(D, K) = Context(S)`, where `D` is a current display data for state `S`, and `K` is a current command set for state `S`. In other words, Symbolback is the one responsible for guiding a user through application communication workflow without the client needing to be aware of application particular semantics.

```
• • • • • • • • • • • • • • • • • • • •
•            Symbolfront              •
• • • • • • • • • • • • • • • • • • • •
          ▲                     •
          •                     •
          •                     •
• • • • • • • • • • •     • • • • • • •
•   display data,   •     •  command  •
•   command list    •     • • • • • • •
• • • • • • • • • • •           •
          •                     •
          •                     •
          •                     ▼
• • • • • • • • • • • • • • • • • • • •
•             Symbolback              •
• • • • • • • • • • • • • • • • • • • •

```

The client doesn't need to understand the transition function `σ`. It only needs to: display `D`, expose `K`, collect/select a command `c ∈ K`, and send `c` back.

## Summary

Symbolfront deliberately trades richer UI expressiveness for simplicity of appearance. The purpose of this project is to explore how far that trade-off can be taken. Richer UIs remain the better interface for many applications, but the question is how simple an interface can become while still supporting an interesting class of applications.
