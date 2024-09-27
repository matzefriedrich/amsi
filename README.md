# Antimalware Scan Interface for .NET

This is a .NET 8.0 library project providing functionality to integrate the [Microsoft Windows Antimalware Scan Interface (AMSI)](https://learn.microsoft.com/en-us/windows/win32/amsi/antimalware-scan-interface-portal?redirectedfrom=MSDN) into any .NET application.

## Usage

Scan a string for malware in C#.

```csharp
const string appName = "myapp";
using (AmsiContext context = AmsiContext.Create(appName))
{
    const string input = "Pure air";
    AmsiScanResult result = context.Scan(input, "");
    if (result == AmsiScanResult.Clean)
    {
        // seems to be okay
    }
}
```

Scanning a buffer full of content for malware is as easy as scanning a `string`; use the overload that accepts a `byte` array.

```csharp
MemoryStream stream = ...
byte[] buffer = stream.ToArray();
AmsiScanResult result = context.Scan(buffer, "");
```

It is also possible to perform correlated scan requests. In the following example, the `ScanFile` method is used to scan file contents for malware.

```csharp
using (AmsiSession scanSession = AmsiSession.Create(context))
{
    string[] files = Directory.GetFiles(...);
    foreach (string file in files)
    {
        AmsiScanResult fileResult = scanSession.ScanFile(file)
        if (fileResult == AmsiScanResult.Block)
        {
            // this file should be blocked...
        }
    }
}
```