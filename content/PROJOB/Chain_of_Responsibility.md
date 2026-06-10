wzorzec, który pozwala przekazywać obsługę operacji do następnych handlerów;

Jeśli umiesz obsłuzyc => robisz
Nie umiesz => dajesz następnemu

```
// Bazowy handler
abstract class InputHandler
{
    private InputHandler _next;

    public InputHandler SetNext(InputHandler next)
    {
        _next = next;
        return next; // pozwala na chainowanie: a.SetNext(b).SetNext(c)
    }

    public virtual void Handle(ConsoleKey key, Player player)
    {
        _next?.Handle(key, player);
    }
}

// Konkretne handlery
class MovementHandler : InputHandler
{
    public override void Handle(ConsoleKey key, Player player)
    {
        switch (key)
        {
            case ConsoleKey.W: player.Y--; return;
            case ConsoleKey.S: player.Y++; return;
            case ConsoleKey.A: player.X--; return;
            case ConsoleKey.D: player.X++; return;
        }
        base.Handle(key, player); // nie mój klawisz — przekaż dalej
    }
}

class InventoryHandler : InputHandler
{
    public override void Handle(ConsoleKey key, Player player)
    {
        if (key == ConsoleKey.I)
        {
            Console.WriteLine("Otwieram inventory...");
            return;
        }
        base.Handle(key, player);
    }
}

class PauseHandler : InputHandler
{
    public override void Handle(ConsoleKey key, Player player)
    {
        if (key == ConsoleKey.Escape)
        {
            Console.WriteLine("Pauza");
            return;
        }
        base.Handle(key, player);
    }
}


```
