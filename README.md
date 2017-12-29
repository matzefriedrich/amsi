# Antimalware Scan Interface for .NET

This is a .NET Standard library project providing functionality to integrate the Microsoft Windows Antimalware Scan Interface (AMSI) into any .NET application. Please find the AMSI documentation at: https://msdn.microsoft.com/en-us/library/windows/desktop/dn889587(v=vs.85).aspx

## Usage

Scan a string for malware in C#.

```csharp
const string appName = "myapp";
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

Scanning a buffer-full of content for malware is as easy as scanning a string; just use the overload that accepts a byte array.

```csharp
MemoryStream stream = ...
const string appName = "myapp";
using (AmsiContext sut = AmsiContext.Create(appName))
{
    byte[] buffer = stream.ToArray();
    AmsiScanResult result = sut.Scan(buffer, "");
    if (result == AmsiScanResult.Clean)
    {
        // seems to be okay
    }
}
```