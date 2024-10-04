#Comportamiento / Transporte / Extensibilidad y Mantenibilidad
El código simula un operador de transporte que puede ejecutar acciones en un vehículo (como arrancar el motor, cargar pasajeros, recargar combustible y detener el motor) de forma ordenada. Las acciones sobre el vehículo se encapsulan en comandos que pueden ser ejecutados de forma flexible y extensible.
<img width="620" alt="image" src="https://github.com/user-attachments/assets/150a8304-e315-423a-8f59-c005c38e0bd9">

https://dotnetfiddle.net/pV7IGV

'''csharp
using System;
using System.Collections.Generic;

// 1. Interfaz Command
// Esta interfaz define el metodo Execute() que debe ser implementado por todos los comandos concretos.
// Cada comando debe ejecutar una accion especifica sobre un receptor (un vehiculo en este caso).
public interface ITransportCommand
{
    void Execute();
}

// 2. Receptor
// La clase Vehicle representa un vehiculo. Contiene diferentes acciones que pueden ser realizadas en el vehiculo,
// como arrancar el motor, detenerlo, recargar combustible, o cargar pasajeros.
// Esta clase actua como el "receptor" en el patron Command.
public class Vehicle
{
    public string Name { get; }

    public Vehicle(string name)
    {
        Name = name;
    }

    // Metodo que simula arrancar el motor del vehiculo
    public void StartEngine()
    {
        Console.WriteLine($"{Name}: El motor ha sido arrancado.");
    }

    // Metodo que simula detener el motor del vehiculo
    public void StopEngine()
    {
        Console.WriteLine($"{Name}: El motor ha sido apagado.");
    }

    // Metodo que simula recargar combustible
    public void Refuel()
    {
        Console.WriteLine($"{Name}: El vehiculo esta siendo recargado con combustible.");
    }

    // Metodo que simula cargar pasajeros en el vehiculo
    public void LoadPassengers(int count)
    {
        Console.WriteLine($"{Name}: Se han cargado {count} pasajeros.");
    }
}

// 3. Comandos concretos
// Cada clase de comando implementa la interfaz ITransportCommand y contiene una referencia a un objeto Vehicle.
// Estos comandos se encargan de ejecutar las acciones especificas llamando a los metodos del receptor (Vehicle).

// Comando concreto para arrancar el motor
public class StartEngineCommand : ITransportCommand
{
    private readonly Vehicle _vehicle;

    public StartEngineCommand(Vehicle vehicle)
    {
        _vehicle = vehicle;
    }

    // Ejecuta la accion de arrancar el motor
    public void Execute()
    {
        _vehicle.StartEngine();
    }
}

// Comando concreto para detener el motor
public class StopEngineCommand : ITransportCommand
{
    private readonly Vehicle _vehicle;

    public StopEngineCommand(Vehicle vehicle)
    {
        _vehicle = vehicle;
    }

    // Ejecuta la accion de detener el motor
    public void Execute()
    {
        _vehicle.StopEngine();
    }
}

// Comando concreto para recargar combustible
public class RefuelCommand : ITransportCommand
{
    private readonly Vehicle _vehicle;

    public RefuelCommand(Vehicle vehicle)
    {
        _vehicle = vehicle;
    }

    // Ejecuta la accion de recargar combustible
    public void Execute()
    {
        _vehicle.Refuel();
    }
}

// Comando concreto para cargar pasajeros
public class LoadPassengersCommand : ITransportCommand
{
    private readonly Vehicle _vehicle;
    private readonly int _passengerCount;

    public LoadPassengersCommand(Vehicle vehicle, int passengerCount)
    {
        _vehicle = vehicle;
        _passengerCount = passengerCount;
    }

    // Ejecuta la accion de cargar pasajeros
    public void Execute()
    {
        _vehicle.LoadPassengers(_passengerCount);
    }
}

// 4. Invocador
// La clase TransportOperator actua como el invocador que almacena los comandos y los ejecuta en un orden especifico.
// Puede agregar comandos a una lista y luego ejecutarlos todos cuando sea necesario.
public class TransportOperator
{
    private readonly List<ITransportCommand> _commands = new List<ITransportCommand>();

    // Metodo para agregar un comando a la lista
    public void SetCommand(ITransportCommand command)
    {
        _commands.Add(command);
    }

    // Metodo para ejecutar todos los comandos almacenados en la lista
    public void ExecuteCommands()
    {
        foreach (var command in _commands)
        {
            command.Execute(); // Se ejecuta cada comando
        }

        _commands.Clear(); // Se limpia la lista despues de ejecutar los comandos
    }
}

// 5. Cliente
// La clase Program es el punto de entrada de la aplicacion.
// Aqui se crean los objetos, se asignan los comandos, y se ejecutan las acciones en el vehiculo.
public class Program
{
    public static void Main(string[] args)
    {
        // Crear el receptor (un vehiculo)
        Vehicle bus = new Vehicle("Autobus");

        // Crear los comandos, cada uno asignado al vehiculo
        ITransportCommand startEngine = new StartEngineCommand(bus);
        ITransportCommand loadPassengers = new LoadPassengersCommand(bus, 20);
        ITransportCommand refuel = new RefuelCommand(bus);
        ITransportCommand stopEngine = new StopEngineCommand(bus);

        // Crear el invocador
        TransportOperator operatorTransport = new TransportOperator();

        // Asignar comandos al invocador
        operatorTransport.SetCommand(startEngine);
        operatorTransport.SetCommand(loadPassengers);
        operatorTransport.SetCommand(refuel);
        operatorTransport.SetCommand(stopEngine);

        // Ejecutar todos los comandos
        operatorTransport.ExecuteCommands();
    }
}


