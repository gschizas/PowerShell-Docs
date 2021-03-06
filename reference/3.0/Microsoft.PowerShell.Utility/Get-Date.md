---
ms.date:  2017-06-09
schema:  2.0.0
locale:  en-us
keywords:  powershell,cmdlet
online version:  http://go.microsoft.com/fwlink/?LinkID=113313
external help file:  Microsoft.PowerShell.Commands.Utility.dll-Help.xml
title:  Get-Date
---

# Get-Date

## Synopsis
Gets the current date and time.

## Syntax

### Net (Default)
```PowerShell
Get-Date [[-Date] <DateTime>] [-Year <Int32>] [-Month <Int32>] [-Day <Int32>] [-Hour <Int32>] [-Minute <Int32>]
 [-Second <Int32>] [-Millisecond <Int32>] [-DisplayHint <DisplayHintType>] [-Format <String>]
 [<CommonParameters>]
```

### UFormat
```PowerShell
Get-Date [[-Date] <DateTime>] [-Year <Int32>] [-Month <Int32>] [-Day <Int32>] [-Hour <Int32>] [-Minute <Int32>]
 [-Second <Int32>] [-Millisecond <Int32>] [-DisplayHint <DisplayHintType>] [-UFormat <String>]
 [<CommonParameters>]
```

## Description
The `Get-Date` cmdlet gets a **DateTime** object that represents the current date or a date that you specify.
It can format the date and time in several Windows and UNIX formats.
You can use `Get-Date` to generate a date or time character string, and then send the string to other cmdlets or programs.

## Examples

### Example 1

```PowerShell
Get-Date -DisplayHint Date
```

```
Tuesday, June 13, 2006
```

This command gets a **DateTime** object, but it displays only the date.
It uses the `-DisplayHint` parameter to indicate that only the date is to be displayed.

### Example 2

```PowerShell
Get-Date -Format g
```

```
6/13/2006 12:43 PM
```

This command gets the current date and time and formats it in short-date and short-time format.
It uses the .NET Framework "`g`" format specifier (General \[short date and short time\]) to specify the format.

### Example 3

```PowerShell
Get-Date -UFormat "%Y / %m / %d / %A / %Z"
```

```
2006 / 06 / 13 / Tuesday / -07
```

This command gets the current date and time and formats it as specified by the command.
In this case, the format includes the full year (`%Y`), the two-digit numeric month (`%m`), the date (`%d`), the full day of the week (`%A`), and the offset from UTC ("Zulu").

### Example 4

```PowerShell
(Get-Date -Year 2000 -Month 12 -Day 31).DayOfYear
```

```
366
```

This command displays the day of the year for the current date.
For example, December 31 is the 365th day of 2006, but it is the 366th day of 2000.

### Example 5
```PowerShell
$a = Get-Date
$a.IsDaylightSavingTime()
```

```
True
```

These commands tell you whether the current date and time are adjusted for daylight savings time in the current locale.

The first command creates a variable named $a and then assigns the object retrieved by `Get-Date` to the $a variable.
Then, it uses the **IsDaylightSavingTime** method on the object in `$a`.

To see the properties and methods of the **DateTime** object, type: `Get-Date | Get-Member`

### Example 6

```PowerShell
$a = Get-Date
$a.ToUniversalTime()
```

```
Tuesday, June 13, 2006 8:09:19 PM
```

These commands convert the current date and time to UTC time.

The first command creates a variable named $a and then assigns the object retrieved by `Get-Date` to the $a variable.
Then, it uses the **ToUniversalTime** method on the object in $a.

### Example 7

```PowerShell
$a = Get-WmiObject Win32_Bios -Computer Server01
$a | Format-List -Property Name, @{Label="BIOS Age";Expression={(Get-Date) - $_.ConvertToDateTime($_.ReleaseDate)}}
```

```
Name     : Default System BIOS
BIOS Age : 1345.17:31:07.1091047
```

Windows Management Instrumentation (WMI) uses a different date-time object than the .NET Framework date-time object that `Get-Date` returns.
To use date-time information from WMI in a command with date-time information from `Get-Date`, you have to use the **ConvertToDateTime** method to convert WMI **CIM_DATETIME** objects to .NET Framework **DateTime** objects.

The commands in this example display the name and age of the BIOS on a remote computer, Server01.

The first command uses the `Get-WmiObject` cmdlet to get an instance of the **Win32_BIOS** class on Server01 and then stores it in the `$a` variable.

The second command uses the pipeline operator (`|`) to send the WMI object stored in $a to the Format-List cmdlet.
The `-Property` parameter of `Format-List` specifies two properties to display in the list, "Name" and "BIOS Age".
The "BIOS Age" property is specified in a hash table.
The table includes the **Label** key, which specifies the name of the property, and the **Expression** key, which contains the expression that calculates the BIOS age.
The expression uses the **ConvertToDateTime** method to convert each instance of ReleaseDate to a .NET Framework DateTime object.
Then, the value is subtracted from the value of the `Get-Date` cmdlet, which, without parameters, gets the current date.

The backtick character (`` ` ``) is the line continuation character in Windows PowerShell.

### Example 8

```PowerShell
Get-Date
```

```
Tuesday, June 13, 2006 12:43:42 PM
```

This command gets a **DateTime** object and displays the current date and time in the long date and long time formats for the system locale, as though you typed "`Get-Date -Format F`".

### Example 9

```PowerShell
Get-Date
```
```
Tuesday, September 26, 2006 11:25:31 AM
```

```PowerShell
(Get-Date).ToString()
```

```
9/26/2006 11:25:31 AM
```

```PowerShell
Get-Date | Add-Content Test.txt

# Adds 9/26/2006 11:25:31 AM
```

```PowerShell
Get-Date -Format F | Add-Content Test.txt

# Adds Tuesday, September 26, 2006 11:25:31 AM
```

These commands demonstrate how to use `Get-Date` with `Add-Content` and other cmdlets that convert the **DateTime** object that `Get-Date` generates to a string.

The first command shows that the default display from a "`Get-Date`" command is in long-date and long-time format.

The second command shows that the default display from the `ToString()` method of the **DateTime** object is in short-date and short-time format.

The third command uses a pipeline operator to send the **DateTime** object to the `Add-Content` cmdlet, which adds the content to the Test.txt file.
Because `Add-Content` uses the ToString() method of the **DateTime** object, the date that is added is in short-date and short-time format.

The fourth command uses the `-Format` parameter of `Get-Date` to specify the format.
When you use the `-Format` or `-UFormat` parameters, `Get-Date` generates a string, not a **DateTime** object.
Then, when you send the string to `Add-Content`, it adds the string to the Test.txt file without changing it.

### Example 10
```PowerShell
Get-Date -Format o
```

```
2012-03-08T10:55:55.6083839-08:00
```

```PowerShell
$timestamp = Get-Date -Format o | foreach {$_ -replace ":", "."}
```

```PowerShell
mkdir C:\ps-test\$timestamp
```

```
    Directory: C:\ps-test

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----          3/8/2012  11:01 AM            2012-03-08T11.00.24.4192623-08.00
```

The first command uses the `-Format` parameter with a value of "`o`" to generate a timestamp string.

The second command prepares the timestamp to be used in a directory name. The command replaces the colon characters (`:`) in the string with dots (`.`) and saves the result in the `$timestamp` variable. Replacing the colons prevents the characters that precede each colon from being interpreted as a drive name.

The third command uses the Mkdir function to create a directory with the name in the `$timestamp` variable.
This example shows how to use the `Get-Date` cmdlet to create a timestamp and how to use the timestamp in or as part of a directory name.

## Parameters

### -Date
Specifies a date and time.
By default, `Get-Date` gets the current system date and time.

Type the date in a format that is standard for the system locale, such as dd-MM-yyyy (German \[Germany\]) or MM/dd/yyyy (English \[United States\]).

```yaml
Type: DateTime
Parameter Sets: (All)
Aliases: LastWriteTime

Required: False
Position: 1
Default value: Current date
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Day
Specifies the day of the month that is displayed.
Enter a value from 1 to 31.
The default is the current day.

If you specify a value that is greater than the number of days in the month, Windows PowerShell adds the number of days to the month and displays the result.
For example, "`Get-Date -Month 2 -Day 31`" displays "March 3", not "February 31".

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: Current day
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisplayHint
Determines which elements of the date and time are displayed.

Valid values are:

- **Date**: displays only the date
- **Time**: displays only the time
- **DateTime**: displays the date and time

**DateTime** is the default.
This parameter does not affect the **DateTime** object that `Get-Date` gets.

```yaml
Type: DisplayHintType
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: DateTime
Accept pipeline input: False
Accept wildcard characters: False
```

### -Format
Displays the date and time in the Microsoft .NET Framework format indicated by the format specifier.
Enter a format specifier.
For a list of available format specifiers, see [DateTimeFormatInfo Class](http://go.microsoft.com/fwlink/?LinkId=143638) in the MSDN (Microsoft Developer Network) library.

When you use the `-Format` parameter, Windows PowerShell gets only the properties of the **DateTime** object that it needs to display the date in the format that you specify.
As a result, some of the properties and methods of **DateTime** objects might not be available.

```yaml
Type: String
Parameter Sets: net
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Hour
Specifies the hour that is displayed.
Enter a value from 1 to 23.
The default is the current hour.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: Current hour
Accept pipeline input: False
Accept wildcard characters: False
```

### -Millisecond
Specifies the milliseconds in the date.
Enter a value from 0 to 999.
The default is the current number of milliseconds.

This parameter is introduced in Windows PowerShell 3.0.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: Current milliseconds
Accept pipeline input: False
Accept wildcard characters: False
```

### -Minute
Specifies the minute that is displayed.
Enter a value from 1 to 59.
The default value is the current minutes.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: Current minutes
Accept pipeline input: False
Accept wildcard characters: False
```

### -Month
Specifies the month that is displayed.
Enter a value from 1 to 12.
The default is the current month.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: Current month
Accept pipeline input: False
Accept wildcard characters: False
```

### -Second
Specifies the second that is displayed.
Enter a value from 1 to 59.
The default is the current second.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: Current seconds
Accept pipeline input: False
Accept wildcard characters: False
```

### -UFormat
Displays the date and time in UNIX format.
For a list of the format specifiers, see the Notes section.

When you use the `-UFormat` parameter, Windows PowerShell gets only the properties of the **DateTime** object that it needs to display the date in the format that you specify.
As a result, some of the properties and methods of **DateTime** objects might not be available.

```yaml
Type: String
Parameter Sets: UFormat
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Year
Specifies the year that is displayed.
Enter a value from 1 to 9999.
The default is the current year.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: Current year
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
This cmdlet supports the common parameters: `-Debug`, `-ErrorAction`, `-ErrorVariable`, `-InformationAction`, `-InformationVariable`, `-OutVariable`, `-OutBuffer`, `-PipelineVariable`, `-Verbose`, `-WarningAction`, and `-WarningVariable`. For more information, see [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216).

## Inputs

### None
You cannot pipe input to this cmdlet.

## Outputs

### System.DateTime or System.String
When you use the `-Format` or `-UFormat` parameters, `Get-Date` returns a string.
Otherwise, it returns a **DateTime** object.

## NOTES
* By default, the date-time is displayed in long-date and long-time formats for the system locale.

  When you pipe a date to cmdlets that expect string input, such as the Add-Content cmdlet, Windows PowerShell converts the **DateTime** object to a string before adding it to the file. The default `ToString()` format is short date and long time. To specify an alternate format, use the `-Format` or `-UFormat` parameters of `Get-Date`.

  * Uformat Values:

    The following are the values of the `-UFormat` parameter. The format for the command is:

    `Get-Date -UFormat %\<value\>`

    For example,
    
    `Get-Date -UFormat %d`

    
     * Date-Time:

       Date and time - full

       (default) : (Friday, June 16, 2006 10:31:27 AM)

       `c` : Date and time - abbreviated (Fri Jun 16 10:31:27 2006)

      * Date:

        `D` : Date in mm/dd/yy format (06/14/06)

        `x` : Date in standard format for locale (09/12/07 for English-US)

      * Year:

        `C` : Century (20 for 2006)

        `Y` : Year in 4-digit format (2006)

        `y` : Year in 2-digit format (06)

        `G` : Same as 'Y'

        `g` : Same as 'y'

      * Month:

        `b` : Month name - abbreviated (Jan)

        `B` : Month name - full (January)

        `h` : Same as 'b'

        `m` : Month number (06)

      * Week:

        `W` : Week of the year (00-52)

        `V` : Week of the year (01-53)

        `U` : Same as 'W'

      * Day:

        `a` : Day of the week - abbreviated name (Mon)

        `A` : Day of the week - full name (Monday)

        `u` : Day of the week - number (Monday = 1)

        `d` : Day of the month - 2 digits (05)

        `e` : Day of the month - digit preceded by a space ( 5)

        `j` : Day of the year - (1-366)

        `w` : Same as 'u'

      * Time:

        `p` : AM or PM

        `r` : Time in 12-hour format (09:15:36 AM)

        `R` : Time in 24-hour format - no seconds (17:45)

        `T` : Time in 24 hour format (17:45:52)

        `X` : Same as 'T'

        `Z` : Time zone offset from Universal Time Coordinate (UTC) (-07)

      * Hour:

        `H` :  Hour in 24-hour format (17)

        `I` :   Hour in 12 hour format (05)

        `k` :  Same as 'H'

        `l` :   Same as 'I' (Upper-case I = Lower-case L)

      * Minutes & Seconds:

        `M` : Minutes (35)

        `S` : Seconds (05)

        `s` : Seconds elapsed since January 1, 1970 00:00:00 (1150451174.95705)

      * Special Characters:

        `n` : newline character (\n)

        `t` : Tab character (\t)


## Related Links

[New-TimeSpan](New-TimeSpan.md)

[Set-Date](Set-Date.md)

