# **Simulation**

[[_TOC_]]

# **SimulationService**

## **Einleitung**
Der _SimulationService_ wurde als Tool, zur einfachen Steuerung und Nutzung einer Simulation, entwickelt.
Die Aufgaben des Services sind:
- Starten eines _SimulationSzenarios_.
- Beenden eines _SimulationSzenarios_.
- Berechnung und Bereitstellung Simulierter Daten auf Anfrage.

## **Interface und Beschreibung**

### **ISimulationService**
```
using System;

namespace Simulation.Library.Models.Interfaces
{
    public interface ISimulationService : IDisposable
    {
        int SimulationScenarioId { get; }
        int MaxEnergyProductionWind { get; }
        int MaxEnergyProductionSun { get; }
        int MaxEnergyConsumption { get; }
        bool IsSimulationRunning { get; }
        TimeSpan Duration { get; }
        DateTime? StartDateTimeReal { get; }
        DateTime? EndDateTimeReal { get; }
        decimal TimeFactor { get; }

        event EventHandler SimulationStarted;
        event EventHandler SimulationEnded;

        void SetSettings(ISimulationServiceSettings settings);
        DateTime? GetSimulatedTimeStamp(DateTime timeStamp);
        int GetEnergyProductionWind(DateTime timeStamp);
        int GetEnergyProductionSun(DateTime timeStamp);
        int GetEnergyConsumption(DateTime timeStamp);
        int GetEnergyBalance(DateTime timeStamp);
        void Run(SimScenario scenario, TimeSpan duration);
        void Stop();
        void SetIdleValues(int energyConsumption, int energyProductionSun, int energyProductionWind);
    }
}
```


### **Properties**

| **Typ** | **Property** | **Zugriff** | **Beschreibung** |
|--|--|--|--|
| int | SimulationScenarioId | { get; } | Gibt die ID des aktuellen Scenarios wieder. Falls kein Scenario läuft wird -1 zurückgegeben. |
| int | MaxEnergyProductionWind | { get; } | Die maximale Energieproduktion die mittels der Windräder produziert werden kann. |
| int | MaxEnergyProductionSun | { get; } | Die maximale Energieproduktion die mittels der Sonnenkollektoren produziert werden kann. |
| int | MaxEnergyConsumption | { get; } | Der maximale Energieverbrauch der von der Stadt verbraucht werden kann. |
| bool | IsSimulationRunning | { get; } | Der Status ob derzeit eine Simulation läuft. |
| TimeSpan | Duration | { get; } | Die Zeitspanne in der die Simulation ablaufen soll. |
| DateTime? | StartDateTimeReal | { get; } | Der tatsächliche Zeitpunkt an dem die Simulation gestartet wurde. |
| DateTime? | EndDateTimeReal | { get; } | Der tatsächliche Zeitpunkt an dem die Simulation beendet wurde. (Errechnet sich aus der Startzeit und der Dauer)|
| decimal | TimeFactor | { get; } | Der Faktor um den die simulierte Zeit schneller/langsamer abläuft als die tatsächliche Zeit. |

### **Events**

| **Event** | **Beschreibung** |
|--|--|
| SimulationStarted | Wird aufgerufen wenn eine Simulation gestartet wird. |
| SimulationEnded | Wird aufgerufen wenn eine Simulation beendet wird. |


### **Constructor**

Zur Initialisierung des Services werden _ISimulationServiceSettings_ benötigt. Diese sind dazu da um initial die Maximalwerte zu setzen. Falls null als Parameter übergeben werden wird für die Maximalwerte der Wert 0 vergeben.

### **Methoden**

| **ReturnType** | **MethodName** | **Parameters** | **Beschreibung** |
|--|--|--|--|
| void | SetSettings | ISimulationServiceSettings settings | Kann verwendet werden um nachträglich die Maximalwerte zu ändern. |
| void | SetIdleValues| int energyConsumption, int energyProductionSun, int energyProductionWind | Kann verwendet werden um Idle-Werte zu setzen. Diese werden zurückgegeben, währenddem keine Simulation läuft. |
| void | Run | SimScenario scenario, TimeSpan duration | Zum Starten einer Simulation. Als Parameter werden ein Szenario und die Dauer benötigt. Wird diese Methode aufgerufen währenddem bereits eine Simulation läuft, wird die bisherige Simulation gestoppt und die neue Simulation wird gestartet. |
| void | Stop |  | Beendet die aktuelle Simulation. |
| void | Dispose |  | Beendet die aktuelle Simulation. |
| DateTime? | GetSimulatedTimeStamp | DateTime timeStamp | Berechnet den Zeitstempel der simulierten Zeit. Als Parameter wird der Zeitpunkt der Anfrage benötigt. |
| int | GetEnergyProductionSun | DateTime timeStamp | Berechnet die aktuelle Energieproduktion der Sonnenkollektoren anhand der Szenario-Daten. Als Parameter wird der Zeitpunkt der Anfrage benötigt. |
| int | GetEnergyProductionWind  | DateTime timeStamp | Berechnet die aktuelle Energieproduktion der Windräder anhand der Szenario-Daten. Als Parameter wird der Zeitpunkt der Anfrage benötigt. |
| int | GetEnergyConsumption | DateTime timeStamp | Berechnet den aktuelle Energieverbrauch der Stadt anhand der Szenario-Daten. Als Parameter wird der Zeitpunkt der Anfrage benötigt. |
| int | GetEnergyBalance | DateTime timeStamp | Berechnet die aktuelle Energiebilanz anhand der Szenario-Daten. Als Parameter wird der Zeitpunkt der Anfrage benötigt. |

## **Berechnungen**

Um die Werte zwischen _SimScenarioPositions_ zu errechnen wird [**lineare Interpolation**](https://www.bauformeln.de/mathematik/lineare-interpolation/) verwendet. Eine eigene Helper-Klasse wurde erstellt, da öfters eine Interpolation durchgeführt werden muss.

### **Code InterpolationHelper**


```
using System;

namespace Simulation.Library.Calculations
{
    public static class InterpolationHelper
    {
        //Returns searched Value between 2 Function positions
        public static decimal GetValue(long x1, long y1, long x2, long y2, decimal givenX)
        {
            if (x1 == x2 && givenX == x1 && y1 == y2)
            {
                return (int)y1;
            }
            return Math.Round(y1 + ((y2 - (decimal)y1) / (x2 - x1)) * (givenX - x1));
        }

        /// <summary>
        /// Linearly interpolates between a and b by x. Used when the Factor is known but the Value is unknown.
        /// </summary>
        /// <param name="min">The 0% value</param>
        /// <param name="max">The 100% value</param>
        /// <param name="x">The percentage factor where 0.0 is 0% and 1.0 is 100%</param>
        /// <returns>Returns the value of the factor x</returns>
        public static decimal Lerp(decimal min, decimal max, decimal x)
        {
            return min + x * (max - min);
        }

        /// <summary>
        /// Linearly interpolates between a and b by x. Used when the Value is known but the Factor is unknown.
        /// </summary>
        /// <param name="min">The 0% value</param>
        /// <param name="max">The 100% value</param>
        /// <param name="x">The actual value</param>
        /// <returns>Returns the factor of the value x</returns>
        public static decimal InverseLerp(decimal min, decimal max, decimal x)
        {
            return max - min == 0 ? 0 : (x - min) / (max - min);
        }
    }
}
```

### **Berechnung des Zeitfaktors**
$factor = \frac{t_{Simulation}}{t_{Scenario}}$

`_timeFactor = InterpolationHelper.InverseLerp(0, Duration.Ticks, _simScenario.GetDuration().Ticks);`

### **Berechnung des simulierten Zeitstempels**
$t_{SimCurrent}=t_{SimStart} + \frac{(t_{SimEnd} - t_{SimStart})}{(t_{RealEnd} - t_{RealStart})} * (t_{RealCurrent } - t_{RealStart})$


```
public DateTime? GetSimulatedTimeStamp(DateTime timeStamp)
{
    if (StartDateTimeReal == null)
    {
       return null;
    }

    return new DateTime((long)InterpolationHelper.GetValue(StartDateTimeReal.Value.Ticks, 
       _simScenario.StartDate.Value.Ticks, 
       EndDateTimeReal.Value.Ticks, 
       _simScenario.EndDate.Value.Ticks, 
       timeStamp.Ticks));
}
```

### **Berechnung der simulierten Energiewerte (Produktion/Konsum)**
$E_{SimCurrent}=t_{prevPos} + \frac{(E_{nextPos} - E_{prevPos})}{(t_{nextPos} - t_{prevPos})} * (t_{SimCurrent } - t_{prevPos})$

        
```
/// <summary>
/// Calculates the percental simulated energyproduction for the given timeStamp for the wind turbines.
/// </summary>
/// <param name="timeStamp">The real TimeStamp</param>
/// <returns>The percental simulated energyproduction. Returns idle values if no Simulation is running.</returns>
public int GetEnergyProductionWind(DateTime timeStamp)
{
    DateTime? simTimeStamp = update(timeStamp);
    if (simTimeStamp == null)
    {
        return _idleEnergyProductionWind;
    }

    return (int)InterpolationHelper.GetValue(_prevPosition.TimeRegistered.Ticks,
        _prevPosition.WindValue,
        _nextPosition.TimeRegistered.Ticks,
        _nextPosition.WindValue,
        simTimeStamp.Value.Ticks);
}

/// <summary>
/// Calculates the percental simulated energyproduction for the given timeStamp for the suncollectors.
/// </summary>
/// <param name="timeStamp">The real TimeStamp</param>
/// <returns>The percental simulated energyproduction. Returns idle values if no Simulation is running.</returns>
public int GetEnergyProductionSun(DateTime timeStamp)
{
    DateTime? simTimeStamp = update(timeStamp);
    if (simTimeStamp == null)
    {
        return _idleEnergyProductionSun;
    }

    return (int)InterpolationHelper.GetValue(_prevPosition.TimeRegistered.Ticks,
        _prevPosition.SunValue,
        _nextPosition.TimeRegistered.Ticks,
        _nextPosition.SunValue,
        simTimeStamp.Value.Ticks);
}

/// <summary>
/// Calculates the percental simulated energyconsumption of the city for the given timeStamp.
/// </summary>
/// <param name="timeStamp">The real TimeStamp</param>
/// <returns>The percental simulated energyproduction. Returns idle values if no Simulation is running.</returns>
public int GetEnergyConsumption(DateTime timeStamp)
{
    DateTime? simTimeStamp = update(timeStamp);
    if (simTimeStamp == null)
    {
        return _idleEnergyConsumption;
    }

    return (int)InterpolationHelper.GetValue(_prevPosition.TimeRegistered.Ticks,
        _prevPosition.EnergyConsumptionValue,
        _nextPosition.TimeRegistered.Ticks,
        _nextPosition.EnergyConsumptionValue,
        simTimeStamp.Value.Ticks);
}
```

### **Berechnung der simulierten Energiebilanz**

```
/// <summary>
/// Calculates the energy balance for the given timeStamp. Negativ if the Consumption is higher than the production of Sun and Wind.
/// </summary>
/// <param name="timeStamp"></param>
/// <returns>The simulated energy balance. Returns idle values if no Simulation is running.</returns>
public int GetEnergyBalance(DateTime timeStamp)
{
    int windValue = GetEnergyProductionWind(timeStamp) * MaxEnergyProductionWind;
    int sunValue = GetEnergyProductionSun(timeStamp) * MaxEnergyProductionSun;
    int consumptionValue = GetEnergyConsumption(timeStamp) * MaxEnergyConsumption;

    int? balanceValue = windValue + sunValue - consumptionValue;
    if (balanceValue >= 0)
    {
        return (int)InterpolationHelper.InverseLerp(0, MaxEnergyProductionWind + MaxEnergyProductionSun, balanceValue.Value);
    }
    else
    {
        return (int)InterpolationHelper.InverseLerp(0, MaxEnergyConsumption, balanceValue.Value);
    }
}
```
