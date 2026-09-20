# Symbolfront

**A thin, context-driven user interface where applications expose only the commands that are meaningful in the current state.**

## Motivation

In this attempt, we are exploring an experiment where input to a certain class of applications may be reduced to simplistic symbolic commands, contrasting the rich user interfaces built on top of those applications. Considering the current context within an application, symbolic command set may be gradually discovered instead of memorizing the entire application instruction set.

Such reduced user interface (UI) platform is not a replacement to traditional rich UIs because there are still a lot of applications that work better with, for example, keyboard shortcuts and mouse events. But we may also acknowledge that there exists a considerable range of applications whose usability remains acceptable using a concise set of contextual commands exchanged between user and application.

## A Word About Backend

Symbolback is intended to serve as a backend service to Symbolfront. It is a symbolic virtual machine passing through various complex states, sending to Symbolfront only necessary contextual information. Thus, along the contextual display data, currently available commands are passed to Symbolfront as `K = κ(S)` where `S` is the current application state and `κ` is a function returning the available command set.

State transitions are defined as `S' = σ(S, c)` where `S` is the current state, the command `c` is an element of `K` command set, `σ` is a backend state transition function, and `S'` is the resulting state. That way, for proper functioning, client doesn't have to be aware of anything else but the current contextual display data and the current command set.

The client stays very thin, and utilizing Symbolback, it lets the user  gradually discover commands related to the current state. In other words, Symbolback is the one responsible for guiding a user through application communication workflow without the client needing to be aware of application particular semantics.

## Example Session

We bring an example session in interacting with typical Symbolfront application. The client does not contain knowledge of any application commands. Instead, during runtime, the current application context provides a set of commands available in that context. That way, the client remains simple while the backend takes care of specific functionality regarding the context.

When the example session starts, user interface shows:

```
Applications.

    Welcome to applications menu. Please enter a command.

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
    
    Task List
    
    The list is empty.

Commands.

    new-task
    done

>
```

We enter `new-task`. UI responds:

```
Tasks. New task.

    Task

    =>  1. Date and time: <date-time>
        2. Description:   <string>
        3. OK?

Commands.

    cursor
    <date-time>
    reject

>
```

We enter `"2026-09-08, 10:00"`. UI responds:

```
Tasks. New task.

    Task

        1. Date and time: "2026-09-08, 10:00"
    =>  2. Description:   <string>
        3. OK?

Commands.

    cursor
    <string>
    reject

>
```

We enter `"Go to dentist"`. UI responds:

```
Tasks. New task.

    Task

        1. Date and time: "2026-09-08, 10:00"
        2. Description:   "Go to dentist"
    =>  3. OK?

Commands.

    cursor
    accept
    reject

>
```

We enter `accept`. We're back to the tasks menu:

```
Tasks.

    Task List

       Index  Day, date, time                 Description
    -------------------------------------------------------------------------
    =>    1.  Tue, 2026-09-08, 10:00          Go to dentist
    -------------------------------------------------------------------------

Commands.

    cursor
    new-task
    edit-task
    delete-task
    done

>
```

We enter `done`. We're back to the applications menu:

```
Applications.

    Welcome to applications menu. Please enter a command.

Commands.

    email
    tasks
    notes
    calc
    done

>
```

We enter `done`. The session ends.

Shown command sets are not fixed, and are chosen for effective functioning of the example application. Different applications may offer different command sets required by their functionality. That approach may cover surprisingly capable class of applications supported by the client.

## Summary

Symbolfront deliberately trades richer UI expressiveness for simplicity of appearance. The purpose of this project is to explore how far that trade-off can be taken. Richer UIs remain the better interface for many applications, but the question is how simple an interface can become while still supporting an interesting class of applications.
