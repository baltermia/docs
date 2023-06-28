# Memento
**<big>Previous > [[Mediator]]</big>**

---

The Memento pattern has similar intentions like the [[Command]] but a entirely different approach. The Memento saves the state of a object, that can later be restored. It's like a backup. Unlike the Command, where individual actions can be undone or be reapplied in an order, mementos are saved in a collection and are used to restore a single state. You can also think of it as an Image (also commonly used for backups).

Let's take the Text-Editor example from the [[Command]] page as a starting point. We can now create a `EditorSnapshot` class, which simply holds the text of it's `Document`.

```csharp
public class EditorSnapshot
{
	public readonly Document Document;
	
	public EditorSnapshot(Document document)
	{
		this.Document = document;
	}
}
```

We also have to add a few methods to the `Editor` class.

```csharp
public void Restore(EditorSnapshot snapshot)
{
	document = snapshot.Document;
}

public EditorSnapshot MakeSnapshot()
{
	EditorSnapshot snapshot = new(new Document());
	
	snapshot.Document.Text = document.Text; // make sure you copy by value, not reference
	
	return snapshot;
}
```

As mentioned in the code, it's very important that all data in a memento/snapshot is copied by value and not by reference. Otherwise creating it in the first place would be redundant.

When using this new code in our application we would likely have some sort of collection that contains tons of mementos so we can restore them if needed. You can also add some additional metadata to the `EditoSnapshot` model, e.g. `DateTime` so you know when you took the snapshot and can sort snapshots and get your desired one.

---

**<big>Next > [[Null Object]]</big>**