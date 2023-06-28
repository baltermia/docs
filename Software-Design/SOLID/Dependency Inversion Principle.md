# Dependency Inversion Principle
**<big>Previous > [[Interface Segregation Principle]]</big>**

---

The last principle defines

> Classes should depend on abstraction but not on concretion

Again, lets make an example:

```csharp
public class Student
{
	public string Firstname { get; set; }
	public int AgeOfBirth { get; set; }
	public int YearOfGraduation { get; set; }
}

public string GetInformation(Student stu)
{
	string name = stu.Firstname;
	int age = DateTime.Now.Year - stu.YearOfBirth;
	
	return $"{name}, {age} years old";
}
```

The method was made for a school and is used to print student information. But what if we want to also use the same method to print teachers information? And do we need all the data `Sudent` stores, or are only a few properties accessed? Instead of writing two methods, maybe overloading methods, we should've have taken an abstraction object as parameter from the beginning:

```csharp
public class Person
{
	public string Firstname { get; set; }
	public int YearOfBirth { get; set; }
}

public class Student : Person
{
	public int YearOfGraduation { get; set; }
}

public class Teacher : Person
{
	public string Lastname { get; set; }
	public int DayOfEmployment { get; set; }
}

public string GetInformation(Person person)
{
	string name = person.Firstname;
	int age = DateTime.Now.Year - person.YearOfBirth;
	
	return $"{name}, {age} years old";
}
```

We provided the most abstract object possible, it only contains the needed properties and implementations. Normally, implementations should also be hidden, so we should use an interface, but since we don't use methods here, an abstract class is also fine (as only getter and setter are defined).

In the best case, define the classes like this:

```csharp
public interface IPerson
{
	public string Firstname { get; }
	public int YearOfBirth { get; }
}

public class Student : IPerson
{
	public string Firstname { get; set; }
	public int YearOfBirth { get; set; }
	public int YearOfGraduation { get; set; }
}

public class Teacher : IPerson
{
	public string Firstname { get; set; }  
	public string Lastname { get; set; }
	public int YearOfBirth { get; set; }
	public int YearOfEmployment { get; set; }
}
```

We only need two properties, `Firstname` and `YearOfBirth` in the `GetInformation` method and only get the values, we don't set them. Therefore the interface also only defines a `get`ter and no `set`ter for the two properties.

---

**<big>End > [[SOLID]]</big>**