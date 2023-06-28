# Interface Segregation Principle
**<big>Previous > [[Liskov's Substitution Principle]]</big>**

---

This principle basically says that instead of providing an interface with functionality that is never actually used in the specific case, multiple interfaces should be create instead of one general.

Take the following code as an example:
```csharp
public interface IControlable
{
	public void StartMotor();
	public void StopMotor();
	public void StartRotor();
}
```

In some (any) class there are two methods that take such an object:
```csharp
public async Task DriveCarAsync(IControlable car) 
{
	car.StartMotor();
	
	await Task.Delay(10000);
	
	car.StopMotor();
}

public async Task FlyAsync(IControlable plane)
{
	plane.StartMotor();
	plane.StartRotor();
	
	await Task.Delay(10000);
	
	plane.StopMotor();
}
```

`DriveCar` doesn't actually ever use `StartRotor` because it expects the `IControlable` object to be a car implementation, while `Fly` uses all of the methods because it expects `IControlable` to be a plane. The very obvious solution would be to split the currently standing interface:

```csharp
public interface IControlable
{
	public void StartMotor();
	public void StopMotor();
}

public interface IFlyable : IControlable
{
	public void StartRotor();
}
```

We moved the `StartRotor` method into the `IFlyable` interface, which also implements the `IControlable` methods. The `DriveCar` method can stand as it is currently, but we need to change the `plane` parameters type in the `Fly` method to `IFlyable`.

---

**<big>Next > [[Dependency Inversion Principle]]</big>**