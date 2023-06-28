# Mediator
**<big>Previous > [[Iterator]]</big>**

---

The Mediator pattern is used when objects need to communicate with other objects of the same type, while they shouldn't specifically know the objects themselves and/or how many they're talking to. It's so to speak a middleman for objects of the same type.

You can imagine it sort of as the Air traffic control of an airport. Planes need to know whether and where they can land, but they shouldn't directly talk to all other airplanes. The Air Traffic control center is the mediator for those objects.

Let's quickly build a group chatting app.

![[mediator.svg]]

```csharp
public interface IChattingMediator
{
	public void Join(Chatter c);
	public void SendMessage(Chatter sender, string text);
}

public class Chatter
{
	public readonly string Name;
	
	public IChattingMediator Mediator { get; set; }
	
	public Chatter(string name) 
	{
		Name = name;
	}
	
	public void Receive(string message)
	{
		Console.WriteLine($"{Name}'s Chat: '{message}'")
	}
	
	public void Send(string message) =>
		_mediator.SendMessage(this, message);
}

public class GroupChat : IChattingMediator
{
	private ICollection<Chatter> chatters = new List<Chatter>();
	
	public void Join(Chatter c)
	{
		chatters.Add(c);
		c.Mediator = this;
		
		SendMessage(c, "<Joined the group>");
	}
	
	public void SendMessage(Chatter sender, string text)
	{
		foreach (Chatter c in chatters)
		{
			if (c != sender) 
			{
				c.Receive($"[{c.Name}]: {text}")
			}
		}
	}
}
```

`Chatter.Receive` prints to the system console when it receives a message. You might have a GUI or something that would display this, it's up to the specific use-case.

You can then use the GroupChat:

```csharp
// create chatters
Chatter pete = new ("Pete");
Chatter sara = new ("Sara");
Chatter tim = new ("Tim");

// create mediator
IChattingMediator mediator = new GroupChat();

// let chatters join
mediator.Join(pete);
mediator.Join(sara);
mediator.Join(tim);

pete.Send("Hey guys :)");

sara.Send("Hi Pete!");

tim.Send("So nice to meet you.");
```

Console Output:

```
Pete's Chat: '[Sara]: <Joined the group>'
Pete's Chat: '[Tim]: <Joined the group>'
Sara's Chat: '[Tim]: <Joined the group>'

Sara's Chat: '[Pete]: Hey guys :)'
Tim's Chat: '[Pete]: Hey guys :)'

Pete's Chat: '[Sara]: Hi Pete!'
Tim's Chat: '[Sara]: Hi Pete!'

Pete's Chat: '[Tim]: So nice to meet you.'
Sara's Chat: '[Tim]: So nice to meet you.'
```

You might think the mediator has some similarities to the [[Observer]] pattern. And yes, they are quite similar as they both work with subscribers and publishers, but while in the Observer, objects that are subscriber are completely cut off to other subscribers the Mediator is specifically built for subscribers to interact with one-and-another.

---

**<big>Next > [[Memento]]</big>**