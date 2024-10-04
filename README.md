#Comportamiento / Transporte / Extensibilidad y Mantenibilidad

El código simula un operador de transporte que puede ejecutar acciones en un vehículo (como arrancar el motor, cargar pasajeros, recargar combustible y detener el motor) de forma ordenada. Las acciones sobre el vehículo se encapsulan en comandos que pueden ser ejecutados de forma flexible y extensible.
<img width="620" alt="image" src="https://github.com/user-attachments/assets/150a8304-e315-423a-8f59-c005c38e0bd9">

https://dotnetfiddle.net/pV7IGV

'''csharp
using System;
using System.Collections.Generic;

public interface ITransportCommand
{
    void Execute();
}

public class Vehicle
{
    public string Name { get; }

    public Vehicle(string name)
    {
        Name = name;
    }

    public void StartEngine()
    {
        Console.WriteLine($"{Name}: El motor ha sido arrancado.");
    }

    public void StopEngine()
    {
        Console.WriteLine($"{Name}: El motor ha sido apagado.");
    }

    public void Refuel()
    {
        Console.WriteLine($"{Name}: El vehiculo esta siendo recargado con combustible.");
    }

    public void LoadPassengers(int count)
    {
        Console.WriteLine($"{Name}: Se han cargado {count} pasajeros.");
    }
}

public class StartEngineCommand : ITransportCommand
{
    private readonly Vehicle _vehicle;

    public StartEngineCommand(Vehicle vehicle)
    {
        _vehicle = vehicle;
    }

    public void Execute()
    {
        _vehicle.StartEngine();
    }
}

public class StopEngineCommand : ITransportCommand
{
    private readonly Vehicle _vehicle;

    public StopEngineCommand(Vehicle vehicle)
    {
        _vehicle = vehicle;
    }

    public void Execute()
    {
        _vehicle.StopEngine();
    }
}

public class RefuelCommand : ITransportCommand
{
    private readonly Vehicle _vehicle;

    public RefuelCommand(Vehicle vehicle)
    {
        _vehicle = vehicle;
    }

    public void Execute()
    {
        _vehicle.Refuel();
    }
}

public class LoadPassengersCommand : ITransportCommand
{
    private readonly Vehicle _vehicle;
    private readonly int _passengerCount;

    public LoadPassengersCommand(Vehicle vehicle, int passengerCount)
    {
        _vehicle = vehicle;
        _passengerCount = passengerCount;
    }

    public void Execute()
    {
        _vehicle.LoadPassengers(_passengerCount);
    }
}

public class TransportOperator
{
    private readonly List<ITransportCommand> _commands = new List<ITransportCommand>();

    public void SetCommand(ITransportCommand command)
    {
        _commands.Add(command);
    }

    public void ExecuteCommands()
    {
        foreach (var command in _commands)
        {
            command.Execute();
        }

        _commands.Clear();
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        Vehicle bus = new Vehicle("Autobus");

        ITransportCommand startEngine = new StartEngineCommand(bus);
        ITransportCommand loadPassengers = new LoadPassengersCommand(bus, 20);
        ITransportCommand refuel = new RefuelCommand(bus);
        ITransportCommand stopEngine = new StopEngineCommand(bus);

        TransportOperator operatorTransport = new TransportOperator();

        operatorTransport.SetCommand(startEngine);
        operatorTransport.SetCommand(loadPassengers);
        operatorTransport.SetCommand(refuel);
        operatorTransport.SetCommand(stopEngine);

        operatorTransport.ExecuteCommands();
    }
}


