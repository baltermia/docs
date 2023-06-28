# Command
**<big>Previous > [[Chain of Responsibility]]</big>**

---

The Command pattern adds Undo-/Redo-Like functionality to a object. Instead of simply defining a method in a class, you define the functionality in another class which also defines a method that can return to a prior state. These commands can then be undone or reapplied again.

It's hard to explain this pattern without visuals, so let's quickly build a simple application.

![[command.svg]]

First we define the basic types.
```csharp
public class Document
{
	public string Text { get; set; }
}

public interface ITextWriter
{
	public void Write(char c);
	public char? Remove();
}

public interface ICommand
{
	public ITextWriter TextWriter { set; }
}
```

Then we can create our concrete `TextWriter`.

```csharp
public class TextWriter : ITextWriter
{
    private readonly Document _document;

    public TextWriter(Document document)
    {
        _document = document;
    }

    public void Write(char c)
    {
        _document.Text += c;
    }

    public char? Remove()
    {
        string text = _document.Text;

        int length = text.Length;

        if (length == 0)
        {
            return null;
        }

        char removed = _document.Text.Last();

        _document.Text = text.Remove(length - 1);

        return removed;
    }
}
```

Now we can define or `Editor`.

```csharp
public class Editor
{
    private readonly IList<ICommand> commands = new List<ICommand>();
    private readonly ITextWriter textWriter;
    private readonly Document document;

    public Editor()
    {
        textWriter = new TextWriter.TextWriter(document = new Document());
    }

    public Editor Action(ICommand command)
    {
        command.TextWriter = textWriter;

        commands.Add(command);

        command.Execute();

        return this;
    }

    public Editor CtrlZ()
    {
        if (!commands.Any())
        {
            return this;
        }

        ICommand last = commands.Last();

        last.Undo();

        commands.Remove(last);

        return this;
    }

    public override string ToString() =>
        document.Text;
}
```

Note we also override `ToString` so we can receive the Editors content later. We also used Fluent API again for the methods (you hopefully remember it from the [[Builder]]).

What's left are the concrete `Commands`.

```csharp
public class RemoveCommand : ICommand
{
    private ITextWriter _textWriter;
    private char? removed;

    public ITextWriter TextWriter { set => _textWriter = value; }

    public void Execute()
    {
        removed = _textWriter.Remove();
    }

    public void Undo()
    {
        if (removed is char c)
        {
            _textWriter.Write(c);
        }
    }
}

public class WriteCommand : ICommand
{
    private ITextWriter _textWriter;
    private readonly char character;

    public ITextWriter TextWriter { set => _textWriter = value; }

    public WriteCommand(char c)
    {
        character = c;
    }

    public void Execute()
    {
        _textWriter.Write(character);
    }

    public void Undo()
    {
        _textWriter.Remove();
    }
}
```

Now the most tedious work is done. Yes, it's a lot of code, but we can use it very simple now:

```csharp
Editor.Editor editor = new();

editor.Action(new WriteCommand('a'))
      .Action(new WriteCommand('b'))
      .Action(new RemoveCommand())
      .CtrlZ()
      .Action(new RemoveCommand())
      .Action(new WriteCommand('c'))
      .Action(new WriteCommand('d'))
      .CtrlZ();

Console.WriteLine(editor);
```

Steps:
```c
a		// write
ab		// write
a		// remove
ab		// undo
a		// remove
ac		// write
acd		// write
ac		// undo - console output
```

---

**<big>Next > [[Iterator]]</big>**