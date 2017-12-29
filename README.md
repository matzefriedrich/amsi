# Antimalware Scan Interface for .NET

This is a .NET Standard library project providing functionality to integrate the Microsoft Windows Antimalware Scan Interface (AMSI) into any .NET application.

## Usage

```csharp
const string appName = "amsitest";
using (AmsiContext sut = AmsiContext.Create(appName))
{
    const string input = "Pure air";
    AmsiScanResult result = sut.Scan(input, "");
    if (result == AmsiScanResult.Clean)
    {
        // seems to be okay
    }
}
```