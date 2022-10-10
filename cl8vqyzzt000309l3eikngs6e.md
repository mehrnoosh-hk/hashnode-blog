# Command Design Pattern And Its Implementation In Python

The Command design pattern is a behavioral pattern that converts a request into a stand-alone object that contains all the information needed for performing the request. Using this approach has the following benefits:
- The request can be passed as an argument to other functions and methods.
- The request can be queued and executed later.
- Provides support for Redo and Undo logic, if applicable.

- Provides support for issuing command on one device and running it on another device also known as networking. (for example in online games)


To better demonstrate this concept, consider that you are building a text editor. Your app has a user interface for communicating with users. This GUI has some buttons for executing commands like copy, paste, save,... How can we implement this using object-oriented design? Perhaps what comes to mind is that you create a Button base class and a bunch of sub-classes like CancelButton, SaveButton, CopyButton, and so on and then implement the actual logic inside each of them. But what is wrong with this approach? Soon you recognize that you have so many sub-classes, but that is not all. The main problem arises when you add other UI components to perform the same work, for example, adding an Edit menu on top of the page or adding keyboard shortcuts. In this case, should we implement the logic in each of these objects? Of course, the answer is no, because doing this makes our code error prone due to duplication, and changing the logic becomes a headache since you have to change so many different places. 
### Command Design Pattern
The solution is to create a command interface. This interface has only one method **execute()**. All business logic commands can be implementations of this interface, and all GUI elements just call the execute method on the relevant command.
[![](https://mermaid.ink/img/pako:eNp1UsuOwjAM_JXIp0XAD0Tcymq1B06IWy5WY5ZqSYJSR6KC_jtp09KHig95zNhjO_EDcqcJJORXLMt9gX8ejbIiWuaMQavF7rndiswTMnXQEu9u1RIrxZrulAemr1Ui0tqmm6o-EtPY2qIhUbIfQTOZOgpNpIYCxFjp7J0RaKsRxG4GzKV75Ul5Yhfb_Dn99u0N6d7Ep6jjxQXPWeDl2BH9SeFANiwHJ2ZopjHYgCFvsNDxY9vHUMAXMqRAxqNG_69A2Tr6hZuOqb51wc6DZB9oAxjYHSub9_fk080GyDNey4hSG3PopqfZ6hfFq7cy)](https://mermaid.live/edit#pako:eNp1UsuOwjAM_JXIp0XAD0Tcymq1B06IWy5WY5ZqSYJSR6KC_jtp09KHig95zNhjO_EDcqcJJORXLMt9gX8ejbIiWuaMQavF7rndiswTMnXQEu9u1RIrxZrulAemr1Ui0tqmm6o-EtPY2qIhUbIfQTOZOgpNpIYCxFjp7J0RaKsRxG4GzKV75Ul5Yhfb_Dn99u0N6d7Ep6jjxQXPWeDl2BH9SeFANiwHJ2ZopjHYgCFvsNDxY9vHUMAXMqRAxqNG_69A2Tr6hZuOqb51wc6DZB9oAxjYHSub9_fk080GyDNey4hSG3PopqfZ6hfFq7cy)

This diagram is maid using [Mermaid](https://mermaid.live/)
### Code example
Let's take advantage of a classic example to demonstrate the command design pattern. Consider that you are designing and building a software system for a bank, and its basic services are depositing, withdrawing, and transferring money from one account to another. This system would be accessed through a mobile app, an ATM device, and a web app.
Lets start with implementing business model.
```Python
from dataclasses import dataclass, field
import random
import string


def create_acc_id():
    return "".join(random.choices(string.digits, k=12))


@dataclass
class Account:
    owner: str
    acc_id: str = field(default_factory=create_acc_id)
    balance: int = 0

    @staticmethod
    def is_valid_deposite_amount(amount: float) -> bool:
        return isinstance(amount, float) and amount >= 0

    def is_valid_withdraw_amount(self, amount: float) -> bool:
        return isinstance(amount, float) and amount < self.balance

    def deposit(self, amount: float) -> None:
        if not self.is_valid_deposite_amount(amount):
            raise ValueError("You can only deposit none negative amount")
        self.balance += amount

    def withdraw(self, amount: float) -> None:
        if not self.is_valid_withdraw_amount(amount):
            raise ValueError(f"You can not withdraw ${amount}")import click

from app.account import Account


@click.group()
def cli():
    pass

@click.command()
@click.argument('owner')
def create_account(owner):
    account = Account(owner)
    click.echo(account.acc_id)
    return account.acc_id



cli.add_command(create_account)

if __name__ == "__main__":
    cli()
        self.balance -= amount

class Bank:
    def __init__(self) -> None:
        self.__accounts = {}
        self.__archived = {}

    def open_account(self, account: Account) -> None:
        if account.acc_id in self.__accounts:
            raise ValueError("This account already exists")
        self.__accounts.update({account.acc_id: account})

    def close_account(self, account: Account) -> None:
        if not account.acc_id in self.__accounts:
            raise ValueError("No such account")
        del self.__accounts[account.acc_id]
        self.__archived.update({account.acc_id: account})
```
So we have two main class an Account and a Bank. To create user interface, I used click and click_shell libraries to create a nice and easy CLI for our Bank-System.

```Python
import click
from click_shell import shell

from app.account import Account
from app.bank import my_bank


@shell(prompt='bank > ', intro="Welcome to our bank app, \nEnter a command "
                               "or type help for instruction")
def bank():
    pass


@bank.command()
@click.option("-owner", required=True, help="The account owner name")
def create_account(owner) -> None:
    """Create New Bank Account

    Args:
        owner (str): The name of the account owner
    """
    account = Account(owner)
    my_bank.open_account(account)
    print(f"Account created with Id {account.acc_id}")


@bank.command()
@click.option("-acc_id", required=True, help="The account id")
@click.option("-amount", required=True, help="The amount to deposit",
              type=click.FLOAT)
def deposit(acc_id: str, amount: float) -> None:
    """Deposit some amount of money to a bank account

    Args:
        acc_id (str): Account's ID
        amount (float): The amount of money to be deposited
    """
    account = my_bank.get_account_by_acc_id(acc_id)
    if not account:
        print("There is no account with this account id")
        return
    account.deposit(amount)
    print("Deposited successfully")
    return


@bank.command()
@click.option("-acc_id", required=True, help="The account id")
@click.option("-amount", required=True, help="The amount to withdraw",
              type=click.FLOAT)
def withdraw(acc_id: str, amount: float) -> None:
    """Withdraw some amount of money to a bank account

    Args:
        acc_id (str): Account's ID
        amount (float): The amount of money to be withdrawn
    """
    account = my_bank.get_account_by_acc_id(acc_id)
    if not account:
        print("There is no account with this account id")
        return
    account.withdraw(amount)
    return


@bank.command()
@click.option("-acc_id", required=True, help="The account id")
def account_info(acc_id: str) -> None:
    """
    Show the account information
    Args:
        acc_id (str): Account's ID
    """
    account = my_bank.get_account_by_acc_id(acc_id)
    if not account:
        print("There is no account with this account id")
        return
    print(account)
    return


if __name__ == "__main__":
    bank()
``` 
Using this CLI app, you can interact with the main business logic of our bank system. As you see in the code, cli.py are calling Account and Bank methods directly. This means that if we change our business logic then we should apply these changes in cli.py accordingly and this means tight coupling. Notice that cli.py should know about existance of both classes and know about their respective methods.

So to decouple our interface from our business logic, we need to add another layer to our application.
To do this we create a file icommand.py and in it we create an interface for our bank transactions commands.

```python
from typing import Protocol


class TransactionCommand(Protocol):

    def execute():
        ...
```
Then to implement this interface create a file commands.py and add the following code to it:

```Python
from app.account import Account
from app.bank import my_bank

class CreateCommand:
    def __init__(self, owner: str):
        self.owner = owner
    def execute(self):
        account = Account(self.owner)
        my_bank.open_account(account)
```
I just implemented the account creation command, but obviously the other commands are not so different. (You can access the completed code here)

Now it is enough to change the cli.py file accordingly:
```Python
from app.commands import CreateCommand

@bank.command()
@click.option("-owner", required=True, help="The account owner name")
def create_account(owner) -> None:
    """Create New Bank Account

    Args:
        owner (str): The name of the account owner
    """
    CreateCommand(owner).execute()
```

As you see, by using command design pattern we decoupled our user interface from our business logic but that is not all, now we can easily add other functionality like logging to execute() method without polluting our user interface or our business logic.

### Resources to learn more:

- https://refactoring.guru/design-patterns/command

- [Implementing Undo And Redo With The Command Design Pattern](https://youtu.be/FM71_a3txTo)

- [Command pattern. (2022, May 27). In Wikipedia.](https://en.wikipedia.org/wiki/Command_pattern)






