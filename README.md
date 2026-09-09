# Sysh

**A symbolic shell promoting simplicity in communication between users and applications**

## Motivation

In this attempt, we are exploring an experiment where interaction with a certain class of applications may be potentially simplified in relation to common graphic user interfaces (GUI) built on top of those applications.

Considering the current context within an application, symbolic instructions may be gradually discovered instead of memorizing the entire application instruction set. Being of symbolic nature, the instructions gain a lot of positive features like simplicity, portability, scriptability, and reproducibility.

Because symbolic input/output could potentially carry only necessary communication information, when working with an application, there may be less distractions in a form of notifications, sounds, images, videos, etc. Hopefully, only information related to work of interest remains.

This reduced user interface (UI) platform is not a replacement to traditional GUIs because there are still a lot of applications that work better with graphic environments. But we may also acknowledge that there exists a considerable range of applications whose simplification would benefit from a short and concise set of simple instructions at given moment, exchanged between user and application.

## Example Session

We bring an example session in interacting with typical Sysh application. The client does not contain knowledge of any application commands. Instead, during runtime, the current application context provides a set of commands available in that context, together with their syntax. That way, the client remains simple while the application takes care of specific functionality regarding the context.

When the example session starts, user interface shows:

```
Applications.

Choose a command:

    e-mail
    tasks
    notes
    calc

    done

>
```

We write `tasks` and press [enter]. UI responds:

```
Tasks.

    Date and time         Description
    ------------------------------------------------------------
    ------------------------------------------------------------

Choose a command:

    Cursor          Modify          Exit
    ------------    ------------    ------------
    today           new-task        done
    prev            edit-task
    next            delete-task
    goto            move-task

tasks>
```

We enter `new-task`. UI responds:

```
New task.
    Date and time.

Choose a command:

    <date-time>
    reject

tasks/new-task>
```

We enter `"08/09/2026, 10:00"`. UI responds:

```
New task.
    Description.

Choose a command:

    <string>
    reject

tasks/new-task>
```

We enter `"Go to dentist"`. UI responds:

```
New task.
    Entered data review.

    Date and time: "08/09/2026, 10:00"
    Description:   "Go to dentist"

Choose a command:

    accept
    reject

tasks/new-task>
```

We enter `accept`. We're back to the tasks menu:

```
Tasks.

    Date and time           Description
    ------------------------------------------------------------
 => 08/09/2026, 10:00       Go to dentist
    ------------------------------------------------------------

Choose a command:

    Cursor          Modify          Exit
    ------------    ------------    ------------    ------------
    today           new-task        done
    prev            edit-task
    next            delete-task
    goto            move-task

tasks>
```

We enter `done`. We're back to the applications menu:

```
Applications. Choose a command:

    e-mail
    tasks
    notes
    calc

    done

>
```

We enter `done`. The session ends.

The example command set is chosen for effective functioning of the example application, but it is in no way restricted to shown commands. Thus, different applications may offer different command sets, as their functionality requires. That way, surprisingly capable class of applications may be supported by this client.

## Summary

Sysh deliberately trades GUI expressiveness for simplicity of use. The purpose of this project is to explore how far that trade-off can be taken. GUIs remain the better interface for many applications, but the question is how simple an interface can become while still supporting an interesting class of applications.
