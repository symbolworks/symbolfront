# Symbolfront

**A symbolic frontend client promoting simplicity in communication between users and applications**

## Motivation

In this attempt, we are exploring an experiment where input to a certain class of applications may be reduced to symbolic commands, contrasting the common graphic user interfaces (GUI) built on top of those applications.

Considering the current context within an application, symbolic instructions may be gradually discovered instead of memorizing the entire application instruction set. Being of symbolic nature, the instructions gain a lot of positive features like simplicity, portability, scriptability, and reproducibility.

This reduced user interface (UI) platform is not a replacement to traditional GUIs because there are still a lot of applications that work better with graphic environments. But we may also acknowledge that there exists a considerable range of applications whose simplification would benefit from a short and concise set of simple instructions at given moment, exchanged between user and application.

## Example Session

We bring an example session in interacting with typical Symbolfront application. The client does not contain knowledge of any application commands. Instead, during runtime, the current application context provides a set of commands available in that context, together with their syntax. That way, the client remains simple while the application takes care of specific functionality regarding the context.

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

The example command set is chosen for effective functioning of the example application, but it is in no way restricted to shown commands. Thus, different applications may offer different command sets, as their functionality requires. That way, surprisingly capable class of applications may be supported by the client.

## A Word About Symbolback

Symbolback is intended to serve as a backend service to symbolfront. It is a virtual machine passing through various complex states, sending to symbolfront only necessary contextual information. Thus, along the contextual display data, currently available commands are passed to symbolfront: `K = κ(S)` where `S` is the current application state and `κ` is a function returning the available command set.

State transitions are defined by: `S' = σ(S, c)` where `S` is the current state, `c` command is element of `K` command set, `σ` is a backend state transition function, and `S'` is the resulting state. This way, for proper functioning, client doesn't have to be aware of anything else but the current contextual display data and the current command set.

## Summary

Symbolfront deliberately trades GUI expressiveness for simplicity of use. The purpose of this project is to explore how far that trade-off can be taken. GUIs remain the better interface for many applications, but the question is how simple an interface can become while still supporting an interesting class of applications.
