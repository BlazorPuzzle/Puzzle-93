# Puzzle #93 - Two-Way Binding
Carl and Jeff want to know how to implement two-way binding between a parent and child component.

YouTube Video: https://youtu.be/P6pMh2Gy35s

Blazor Puzzle Home Page: https://blazorpuzzle.com

## The Challenge

This is a .NET 9 Blazor Web App with global server interactivity

We are binding to a component instantiated on the page.

The goal is to have two-way binding between the input field and the component.

When we change the message in the Home page, we want it to update in the component.

Also, when the component changes the message, we want to reflect that change in the home page.

This code looks like it should work, but it doesn't.

How can we fix it?

### The Solution

The solution is to create an `EventCallback` parameter in the component using the convention `[ParameterName]Changed`. 

In this case, it looks like this:

```c#
[Parameter]
public EventCallback<string> ParentMessageChanged { get; set; }
```

The type `<string>` must be of the type of the parameter that is changing: `ParentMessage`.

Then, all we need to do is invoke this `EventCallback` in the `UpdateMessage()` method:

```c#
private async Task UpdateMessage()
{
    ParentMessage = "Blaze On";
    await ParentMessageChanged.InvokeAsync(ParentMessage);
}
```

Now in the home page, we need to use a different syntax to bind to the `ParentMessage` parameter.

Change this:

```c#
<MyComponent ParentMessage="Message" />
```

to this:

```c#
<MyComponent @bind-ParentMessage="Message" />
```

With these two changes in place, two way binding works as expected.

Boom!
