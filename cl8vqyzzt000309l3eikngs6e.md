## Command Design Pattern And Its Implementation In Python

The Command design pattern converts a request into a stand-alone object that contains all the information needed for performing the request. Using this approach has the following benefits:
- The request can be passed as an argument to other functions and methods.
- The request can be queued and executed later.
- Provides support for Redo and Undo logic, if applicable.
To better demonstrate this concept, consider that you are building a text editor. Your app has a user interface for communicating with users. This GUI has some buttons for executing commands like copy, paste, save,... How can we implement this using object-oriented design? Perhaps what comes to mind is that you create a Button base class and a bunch of sub-classes like CancelButton, SaveButton, CopyButton, and so on and then implement the actual logic inside each of them. But what is wrong with this approach? Soon you recognize that you have so many sub-classes, but that is not all. The main problem arises when you add other UI components to perform the same work, for example, adding an Edit menu on top of the page or adding keyboard shortcuts. In this case, should we implement the logic in each of these objects? Of course, the answer is no, because doing this makes our code error prone due to duplication, and changing the logic becomes a headache since you have to change so many different places. 
### Command Design Pattern
The solution is to create a command interface. This interface has only one method **execute()**. All business logic commands can be implementations of this interface, and all GUI elements just call the execute method on the relevant command.

![CommandDesignPattern.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664980758694/Bvs9j9Bpx.png align="center")
### Resources to learn more:

- https://refactoring.guru/design-patterns/command

- [Implementing Undo And Redo With The Command Design Pattern](https://youtu.be/FM71_a3txTo)

- [Command pattern. (2022, May 27). In Wikipedia.](https://en.wikipedia.org/wiki/Command_pattern)






