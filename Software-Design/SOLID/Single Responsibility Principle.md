# Single Responsibility Principle
**<big>Previous > [[SOLID]]</big>**

---

The name already defines this principle.

> A class should be having one and only one responsibility

Let's make an example:
```csharp
public class DbDataStorer
{
	private Data m_data;
	
	public bool ConnectToDatabase(string connectionString) { /* connect to db */ }
	public bool FetchData(string sql) { /* fetch data from db and write into m_data */ }
	public bool WriteToFile(string path) { /* write data from m_data into file under path */ }
}
```
The class does the following things:
- Connect to a Database
- Fetch some data and return it
- Write the fetched data into a file

Now we want to add some other functionality, say create a new database. That wouldn't make any sense to add to the existing `DbDataStorer` class, but creating a new class would also not make any sense because `DbDataStorer` already contains methods that interact with a database.

The simple solution is to follow the single responsibility principle, so we would create a class that each takes one of the tasks listed above as responsibility. Then we can have for example a wrapper class, that uses all the classes that interact with databases, which allows all the database methods to be executed. That class would have the wrapper database responsibility, we wouldn't need to change code inside that class if we modify any of the tasks.

---

**<big>Next > [[Open Closed Principle]]</big>**
