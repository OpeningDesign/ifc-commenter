# IFC Commenter

Generates comments containing the content of references making IFC files easier to manually review.

## Example

```ifc
#71= IFCDERIVEDUNIT((#68,#69,#70),.THERMALTRANSMITTANCEUNIT.,$);
#73= IFCDERIVEDUNITELEMENT(#43,3);
```

**Becomes**:

```ifc
#71= IFCDERIVEDUNIT((#68,#69,#70),.THERMALTRANSMITTANCEUNIT.,$);
  /*#68= IFCDERIVEDUNITELEMENT(#59,1);*/
    /*#59= IFCSIUNIT(*,.MASSUNIT.,.KILO.,.GRAM.);*/
  /*#69= IFCDERIVEDUNITELEMENT(#66,-1);*/
    /*#66= IFCSIUNIT(*,.THERMODYNAMICTEMPERATUREUNIT.,$,.KELVIN.);*/
  /*#70= IFCDERIVEDUNITELEMENT(#64,-3);*/
    /*#64= IFCSIUNIT(*,.TIMEUNIT.,$,.SECOND.);*/
#73= IFCDERIVEDUNITELEMENT(#43,3);
  /*#43= IFCSIUNIT(*,.LENGTHUNIT.,$,.METRE.);*/
```

## Installation

Requires [node](https://nodejs.org) installed.

```sh
npm install --global ifc-commenter

ifc-commenter example.ifc
```

To export out a commented file:

`ifc-commenter example.ifc --output example_commented_.ifc`

The file name(s) should not have any spaces.

## Creating a Windows shell script to right-click on .ifc to save as commented .ifc file
1.  **Create a Batch Script**: First, create a batch script that will take a file as input, run your command, and output the result with the `_commented` suffix.
    
```
    @echo off
    setlocal
    
    rem Get the input file path
    set "inputFile=%~1"
    
    rem Get the directory and filename without extension
    set "directory=%~dp1"
    set "filename=%~n1"
    
    rem Set the output file path
    set "outputFile=%directory%%filename%_commented.ifc"
    
    rem Run the command
    ifc-commenter "%inputFile%" --output "%outputFile%"
    
    endlocal
```
    
2.  **Save the Batch Script**: Save this script with a `.bat` extension, for example, `ifc-commenter-script.bat`.
    
3.  **Add the Script to the Context Menu**: To add this script to the right-click context menu, you need to add an entry to the Windows Registry.
    
    -   Open the Registry Editor (press `Win + R`, type `regedit`, and press Enter).
    -   Navigate to `HKEY_CLASSES_ROOT\*\shell`.
    -   Right-click on `shell`, select `New -> Key`, and name it something like `IFC Commenter`.
    -   Right-click on the new key (`IFC Commenter`), select `New -> Key`, and name it `command`.
    -   Set the value of the `(Default)` entry in the `command` key to the path of your batch script with `%1` as an argument. For example:
        
        `"C:\path\to\your\script\ifc-commenter-script.bat" "%1"` 
        

After these steps, when you right-click on an `.ifc` file, you should see an option called `IFC Commenter` in the context menu. Selecting this option will run the batch script, which in turn will run your command and generate the output file with the `_commented` suffix.