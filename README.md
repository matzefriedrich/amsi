![CI](https://github.com/matzefriedrich/amsi/actions/workflows/dotnet.yml/badge.svg)
![GitHub Tag](https://img.shields.io/github/v/tag/matzefriedrich/amsi)
![GitHub License](https://img.shields.io/github/license/matzefriedrich/amsi)


# Antimalware Scan Interface for .NET

This is a .NET 8.0 library project providing functionality to integrate the [Microsoft Windows Antimalware Scan Interface (AMSI)](https://learn.microsoft.com/en-us/windows/win32/amsi/antimalware-scan-interface-portal?redirectedfrom=MSDN) into any .NET application.

## Build

```sh
$ dotnet build --configuration Release
```

### Run tests

> **The library uses the AMSI interface, which is only available on Windows desktop versions.** You will encounter several failing tests if you run the tests on a non-Desktop version of Windows.

```sh
$ dotnet test --framework net8.0-windows --configuration Release --verbosity normal
```


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