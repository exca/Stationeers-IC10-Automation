# Stationeers-BASIC-IC10
A BASIC compiler for Stationeers MIPS IC10 code, developed in C#, in Unity, by Exca (Fly Game Studio).

-----

This page introduces the BASIC-IC10 programming language designed for the IC10 chips in the game Stationeers.
A BASIC language is well-suited for managing complex code and is particularly accessible to individuals who may not have prior programming experience due to its simple and user-friendly syntax.

The compiler serves as a translator that converts source code written in the BASIC programming language into machine code instructions compatible with the MIPS architecture.
It ensures that the high-level code written by the programmer can be executed by the IC10 chips in the game Stationeers by generating the necessary low-level instructions for the hardware to understand and execute.

The primary goal of this programming language is to provide a comprehensive toolset for scripting and automation within the Stationeers game environment.
Whether you're a seasoned programmer or new to coding, this language opens up exciting for enhancing your Stationeers gaming experience.
This document will guide you through the language's features, syntax, and usage.

# Declarations

## Variables and Devices aliases

A variable declared by VAR is used to store and manipulate data, while a variable declared by ALIAS assigns a user-friendly name to a Pin or device for ease of reference within the programming language.

1. **VAR**
   - *Explanation:* Used to declare and define variables to store data in the BASIC language.
   - *Example:* `VAR temperature`
   - *Example 2:* `VAR temperature = 25.5` (assigning a value is optional)

2. **CONST**
   - *Explanation:* Declares custom constants that store fixed values.
   - *Example:* `CONST MaxTemperature = 30C` (the value is mandatory)

3. **ALIAS**
   - *Explanation:* Assigns a name or alias to a Pin or device for easier reference.
   - *Example:* `ALIAS MySensor Pin0`

The Pin number must be specified between 0 and 5.

## More declaration types

In some situations, users may opt to declare multiple values in an array when dealing with collections of data, or declare multiple devices using batch operations to efficiently manage and manipulate groups of similar devices.

4. **ARRAY**
   - *Explanation:* Declares arrays to store multiple values of the same type.
   - *Example:* `ARRAY sensorData[10]`

5. **BATCH**
   - *Explanation:* Declares a device type using a hash or name for batch operations.
   - *Example:* `BATCH MyDevices StructureArcFurnace` (with the name of the device found in Stationpedia)
   - *Example 2:* `BATCH MyDevices -247344692` (with the Hash of the device found in Stationpedia)

6. **NAME**
   - *Explanation:* Associates a device with its name, using the hash or name of the device type.
   - *Example:* `NAME MyDevice StructureGasSensor "Airlock Sensor"` (with the name of the device found in Stationpedia)
   - *Example 2:* `NAME MyDevices -1252983604 "Airlock Sensor"` (with the Hash of the device found in Stationpedia)

Declaring a device using NAME is a method to manage more than six devices within the constraints of an IC, which typically offers only six available pins, by associating specific devices with their names or types, thereby allowing for the effective management of a larger number of devices in the program.


# Read and Write

## Variables

To write a value into a variable, you can use a simple assignment statement like `variable = value`.

```basic
myVariable = 5
```

## Devices

For setting a variable within a device, you would specify the device and the variable you want to set.

To set a device variable (e.g., Setting of a volume pump):

```basic
MyPump.Setting = 5
```

In the example above, `MyPump` represents the device, and `Setting` is the specific variable within that device that is being set to the value 5.

### Read a slot

To read a slot on a device in the BASIC programming language, you need to specify the device and the slot you want to read. The syntax for reading a slot on a device is as follows:

```
DeviceName[SlotType].SlotProperty
```

- `DeviceName`: This is the name of the device you want to read from.
- `SlotType`: Specify the type of slot you want to access, which can be `Import`, `Export`, or `Content`.
- `SlotProperty`: This is the variable or data you want to retrieve from the slot.

Here's an example of how to read the `OccupantHash` property from an `Import` slot of a device named `MySorter`:

```basic
CurrentHashInSorter = MySorter[Import].OccupantHash
```

In this example, `MySorter` represents the sorter, `[Import]` specifies the slot type as Import, and `.OccupantHash` retrieves the value of the OccupantHash property from the Import slot of the device, the the value is stored in the variable `CurrentHashInSorter`.

You can use similar syntax for other slot types and properties on the device, depending on your specific needs within the game.

Slots in the BASIC programming language can be specified using either a variable or a constant, and there are predefined constants for slot types, including `Import` (slot 0), `Export` (slot 1), and `Content` (slot 2). Check the Stationpedia to get the exact slot number to use.

### Read a reagent quantity

In the BASIC programming language, you can read a reagent on a device using a specific syntax. To read a reagent, you need to specify the "Reagent" variable, followed by the reagent name or reagent hash within square brackets. Here's how to do it:

```basic
Device.Reagent[ReagentName].ReagentProperty
```

- `Device`: This represents the device on which you want to read the reagent.
- `ReagentName`: You specify the name of the reagent you want to access within square brackets.
- `ReagentProperty`: This is the specific property or data you want to retrieve from the reagent.

Here's an example of how to read the `Contents` property of an "Iron" reagent on a device named `MyDevice`:

```basic
MyDevice.Reagent[Iron].Contents
```

In this example, `MyDevice` represents the device, `[Iron]` specifies the reagent name as "Iron," and `.Contents` retrieves the value of the Contents property for the Iron reagent.

You can use similar syntax to read other reagent properties or access different reagents on devices as needed within your program.

Here is a list of the `ReagentName` values that can be found in a printer: `Silicon`, `Iron`, `Gold`, `Copper`, `Silver`, `Lead`, `Steel`, `Electrum`, `Invar`, `Constantan`, `Solder`, `Astroloy`, `Hastelloy`, `Inconel`, `Waspaloy`, `Stellite`, `Nickel`.
Using one of these names will automatically generate the Hash of the Ingot Item.

The `ReagentName` can also be a variable (`VAR`) or a constant (`CONST`) value.

There are 3 different `ReagentProperty` values that can be used:
- `Contents` to return the quantity of the specified reagent inside the printer.
- `Required` to return the missing quantity of the specified reagent to be able to print the selected recipe.
- `Recipe` to return the total quantity of the specified reagent needed to print 1 unit of the selected recipe.

If no `ReagentProperty` is specified, the default `Contents` will be considered.

### Specify batch reading mode

When reading the value of a Batch or Name device in our BASIC programming language, you can specify the batch mode using the following syntax:

```
DeviceType.Variable.BatchMode
```

- `DeviceType`: This is the name or hash of the device type.
- `BatchMode`: Specify the batch mode, which can be `Average`, `Sum`, `Minimum`, or `Maximum`.
- `Variable`: The specific variable or property you want to read within the batch device.

Here's an example of how to read the `Temperature` variable in `Average` mode for a device type named `MyDevices`:

```basic
RoomTemperature = MyDevices.Temperature.Average
```

In this example, `MyDevices` represents the device type, `.Average` specifies the batch mode as Average, and `.Temperature` retrieves the value of the Temperature variable in Average mode.

You can use this syntax to read specific properties or variables within batch devices, while selecting the appropriate batch mode for your desired calculations within the game.


# Values

## Set a value

There are two main types of values in our BASIC programming language:

1. **Boolean Values:** These are used to represent binary states, where `true` stands for "on" or "1," and `false` represents "off" or "0".

For example, to switch a light on, you would assign `true` to a variable or device variable:

```basic
Light.On = true
```

2. **Numeric Values:** Numeric values can be further divided into two subtypes:

- **Integer Values (int):** These are whole numbers, such as -1, 0, 1, 2, and so on.
- **Floating-Point Values (float):** These are numbers with decimal points, like 3.14, -0.5, 100.25, etc.

## Units

Any numeric value can be specified with a unit to perform automatic conversions.
The available units and their conversions are as follows:

Pressures:
- **Pa (Pascal):** Multiply by 0.001 to convert to kPa.
- **kPa (Kilopascal):** Multiply by 1 to convert to kPa.
- **MPa (Megapascal):** Multiply by 1000 to convert to kPa.

Temperatures:
- **K (Kelvin):** No conversion required, it's in absolute temperature.
- **C (Celsius):** Add 273.15 to convert to Kelvin.

Percentage:
- **% (Percentage):** Multiply by 0.01 to convert to the percentage representation.

For example, to set a temperature of 25 degrees Celsius as a constant in Kelvin (which is the default unit in the game):

```basic
CONST temperature = 25C
CONST ratio = 10%
CONST max_pressure = 59.99 MPa
```

The language will automatically convert the value `25C` to Kelvin (298.15K) for internal calculations.

## Strings (Hash representation)

When a string is delimited by double quotes (e.g., `"example"`), it is automatically converted into a hash of that string. This feature can be useful for storing and managing multiple hashes in an array.

A Hash is a compiled value of a string and is used in Stationeers to represent a device on the data network, or an item inside a slot. For example, reading the variable `OccupantHash` will return a value that represent the name of the item. This value can be compared in code.

This code uses string-based hashes to compare and control the behavior of a Sorter based on item present in its Import slot:

```basic
ALIAS Sorter Pin0
Sorter.Mode = 2
Start:
VAR item = Sorter[Import].OccupantHash
IF item != 0 THEN
    Sorter.Output = (item != "ItemCoalOre") # Once compiled, "ItemCoalOre" value is 1724793494
ENDIF
GOTO Start
```

In this code we continuously checks the `OccupantHash` variable of the `Sorter` device.
If the `OccupantHash` value is not equal to 0 (indicating that there's an item in the Import slot), it compares the `OccupantHash` value with the hash of "ItemCoalOre" (which is represented as a string enclosed in double quotes).
If the `OccupantHash` value matches "ItemCoalOre," it sets the `Sorter.Output` variable to `0`, indicating that the Sorter should output the coal ore on the Output 0. Any other item will be sent to the Output 1.

## Arrays of values

To store data in an array in the BASIC programming language, you can use the square brackets `[]` to access specific elements of the array and assign values to them. Here's a basic example of storing data in an array:

```basic
ARRAY myArray[5]  # Create an array with 5 elements

# Store values in the array
myArray[0] = 10
myArray[1] = 20
myArray[2] = 30
myArray[3] = 40
myArray[4] = 50
```

To read data from an array, you can also use square brackets to access the elements and retrieve their values. Here's how to read data from the array:

```basic
VAR value = myArray[2]  # Retrieves the value 30 from myArray[2]
```

In this example, `myArray[2]` is used to access the third element of the array, which contains the value 30. You can use similar syntax to store and retrieve data from arrays in your code.

Arrays can be usefull to store a list of values or Hash:

```basic
ARRAY hashArray[5]  # Create an array to store hashes

# Store strings as hashes in the array
hashArray[0] = "Item1"
hashArray[1] = "Item2"
hashArray[2] = "Item3"
hashArray[3] = "Item4"
hashArray[4] = "Item5"

# Access and use the stored hashes
VAR item
item = hashArray[0]  # item now contains the hash of "Item1"
item = hashArray[2]  # item now contains the hash of "Item3"
```

In this example, we create an array `hashArray` to store hashes. We then assign strings enclosed in double quotes to the array elements. These strings are automatically converted to hashes. Later in the code, you can access and use these hashes for various purposes, such as referencing items in the array.

# Built-in constants

## Colors

In the BASIC programming language, you can specify colors using predefined constants. Here are the available color constants and an example of setting the "Color" variable of a Light device:

Color Constants:
- `Blue = 0`
- `Grey = 1`
- `Green = 2`
- `Orange = 3`
- `Red = 4`
- `Yellow = 5`
- `White = 6`
- `Black = 7`
- `Brown = 8`
- `Khaki / Kaki = 9`
- `Pink = 10`
- `Purple = 11`

Example of setting the "Color" variable of a Light device to "Green" (Color constant 2):

```basic
Light.Color = Green
```

In this example, the `Light` device's "Color" variable is set to the predefined constant `Green`, which corresponds to the color green. This syntax allows you to easily set the color of the device without having to remember the corresponding values.

# Calculations

## Operators:

1. **Arithmetic Operators:**
   - `/` (Division): Divides the value on the left by the value on the right.
   - `*` (Multiplication): Multiplies the values on both sides.
   - `-` (Subtraction): Subtracts the right value from the left value.
   - `+` (Addition): Adds the values on both sides.

2. **Boolean Operators:**
   - `||` (Logical OR): Returns true if either side is true.
   - `&&` (Logical AND): Returns true if both sides are true.

## Comparators:

1. **Value Comparators:**
   - `>` (Greater Than): Checks if the left value is greater than the right.
   - `>=` (Greater Than or Equal To): Checks if the left value is greater than or equal to the right.
   - `==` (Equal To): Checks if the values on both sides are equal.
   - `<=` (Less Than or Equal To): Checks if the left value is less than or equal to the right.
   - `<` (Less Than): Checks if the left value is less than the right.
   - `!=` (Not Equal To): Checks if the values on both sides are not equal.

2. **Boolean Comparators:**
   - `==` (Equal To): Checks if two boolean values are equal.
   - `!=` (Not Equal To): Checks if two boolean values are not equal.

Note: the single equal sign `=` is used only to set a value to a variable/device.

Additionally, you can use parentheses to prioritize calculations, just like in standard mathematical expressions. For example, you can use parentheses to ensure that certain operations are performed before others. For instance:

```basic
result = (2 + 3) * 4
```

In this case, the addition within the parentheses is performed first, and then the result is multiplied by 4, yielding `result = 20`.

# Structures

## Conditional structures

In the BASIC programming language, you can use conditional structures to control the flow of your program based on certain conditions. There are two main conditional structures: `IF...THEN...ENDIF` and `IF...THEN...ELSE...ENDIF`.

1. **IF...THEN...ENDIF:**
   - `IF...THEN...ENDIF` is used for basic conditional branching.
   - Here's the structure:
   
     ```basic
     IF condition THEN
         # Code to execute if the condition is true
     ENDIF
     ```
   - The program checks the `condition`, and if it's true, it executes the code within the `THEN` block. If the `condition` is false, the code inside the `THEN` block is skipped.

2. **IF...THEN...ELSE...ENDIF:**
   - `IF...THEN...ELSE...ENDIF` allows for branching with an alternative action when the condition is false.
   - Here's the structure:
   
     ```basic
     IF condition THEN
         # Code to execute if the condition is true
     ELSE
         # Code to execute if the condition is false
     ENDIF
     ```
   - The program checks the `condition`. If it's true, it executes the code within the first block (`THEN`); if the condition is false, it executes the code within the second block (`ELSE`).

Here's an example of both structures in action:

```basic
ALIAS Sensor Pin0
VAR temperature = Sensor.Temperature

# Declare muliple wall heaters by their Hash
BATCH Heaters StructureWallHeater

IF temperature > 25C THEN
    # This code will execute if the temperature is greater than 25 degrees Celsius.
    Heaters.On = false
ENDIF

IF temperature <= 20C THEN
    # This code will execute if the temperature is 20 degrees Celsius or lower.
    Heaters.On = true
ENDIF

# Declare muliple wall coolers by their Hash
BATCH Coolers StructureWallCooler

IF temperature >= 30C THEN
    # This code will execute if the temperature is 30 degrees Celsius or grater.
    Coolers.On = true
ELSE
    # This code will execute if the temperature is lower than 25 degrees Celsius.
    Coolers.On = false
ENDIF
```

In this example, the first `IF...THEN...ENDIF` structure checks if the temperature is too low and turns on the wall heaters. The second `IF...THEN...ELSE...ENDIF` structure checks if the temperature is too high and switch on the coolers.

Note that the second structure could be replaced by setting the device using an operation in one line:
```basic
Coolers.On = (Sensor.Temperature >= 30C)
```

## Loops structures

In the BASIC programming language, the `WHILE...ENDWHILE` structure is used to create loops that repeatedly execute a block of code as long as a specified condition is true. Here's the structure:

```basic
WHILE condition THEN
    # Code to execute while the condition is true
ENDWHILE
```

- The program checks the `condition` before entering the loop.
- If the `condition` is true, it executes the code within the loop.
- After executing the code in the loop, it returns to the `WHILE` statement and checks the `condition` again. If the condition is still true, the loop continues. If the condition becomes false, the loop terminates, and the program proceeds to the code after the `ENDWHILE`.

Here's an example of a `WHILE...ENDWHILE` loop:

```basic
ALIAS Sensor Pin0
ALIAS RedLight Pin1
WHILE Sensor.Pressure > 50MPa
   # This code will execute as long as the pressure is above 50MPa
   WAIT(1) # Wait for 1 second
   RedLight.On = NOT(RedLight.On) # Blink the reg light
ENDWHILE
```

In this example, the `WHILE` loop checks if the `Pressure` read by the `Sensor` is higher than 50MPa. As long as the condition is true, it will switch on/off the red light every 1 second. The loop continues until `Pressure` is no longer higher than 50MPa, at which point it terminates.

## Jumps: Labels and Goto functions

In the BASIC programming language, labels and the `GOTO` function are used for branching and directing the flow of your program. Here's an explanation of how they work:

1. **Labels:**
   - Labels are used to mark specific points in your code. They act as targets for the `GOTO` function, allowing you to jump to a particular point in the code.
   - Labels are defined with a colon at the end of a line, and they are typically placed before the code block they mark.
   - Example of defining a label:

     ```basic
     myLabel:
     # Code block associated with the label
     ```

2. **GOTO:**
   - The `GOTO` function is used to transfer control to a labeled section of the code. It specifies the label to which the program should jump.
   - Example of using `GOTO` to jump to a label:

     ```basic
     GOTO myLabel
     ```

   - When the `GOTO` function is executed, the program will immediately jump to the labeled section of code marked by `myLabel:`. Execution continues from that point onward.

Here's a practical example of how labels and `GOTO` can be used:

```basic
VAR counter = 0

startLoop:
IF counter < 5 THEN
    # Code to execute
    counter = counter + 1
    GOTO startLoop
ENDIF
```

In this example, the label `startLoop` is defined before the loop begins. The program checks the `counter`, and as long as it's less than 5, it executes the code inside the loop and increments the counter. The `GOTO` function directs the program back to the `startLoop` label to repeat the loop until the condition is no longer met. This pattern creates a loop that runs five times.

Labels and `GOTO` can be helpful for controlling program flow in certain situations, but they should be used judiciously to maintain code readability and avoid unnecessary complexity.

# Functions

## Yield and Wait

In the BASIC programming language, the `Yield` and `Wait` functions are used to control the timing and execution of code. Here's an explanation of each:

1. **yield():**
   - In Stationeers, the Yield instruction pauses execution of the script until the next game tick (0.5s), this is important since other devices will not update until the next tick so using yield will improve the efficiency of your code.
   - Example of using `Yield()` in a loop:
   
     ```basic
     WHILE condition THEN
         # Code to execute
         Yield()  # Introduce a yield to allow devices to update their variables
     ENDWHILE
     ```

   - The compiler will try to place automatically a `Yield` in the main loop of your program to prevent it from consuming excessive resources. However, you can also manually insert `Yield` to control the pace of execution.

2. **wait(duration):**
   - The `Wait()` function is used to introduce a longer waiting time compared to `Yield()`. You can specify the waiting time in seconds as an argument to `Wait()`. This function is helpful when you need precise timing or longer pauses in your code.
   - Example of using `Wait()` to wait for 5 seconds:
   
     ```basic
     Wait(5)  # Wait for 5 seconds
     ```

   - You can use `Wait` to introduce delays, for example, to let time passing in the game or to synchronize actions with specific time intervals.
   - `Wait` can be called using a variable or calulation, for example `WAIT(0.5 * myRatio)`.

Both `Yield()` and `Wait()` functions are essential for managing the timing and responsiveness of your code, especially in scenarios where you want to avoid excessive resource usage or coordinate actions with the game's simulation time.

## Calculation

In the BASIC programming language, there are several built-in functions, including `abs()`, `round()`, and `not()`, designed to perform specific calculation operations. Here's an explanation of these functions:

1. **abs(value):**
   - The `abs()` function is used to calculate the absolute value of a numeric value. It returns the positive magnitude of the number, effectively removing the sign.
   - Example:

     ```basic
     VAR result = abs(-10)  # result will be 10
     ```

2. **round(value):**
   - The `round()` function is used to round a floating-point value to the nearest integer. It performs standard rounding, rounding up if the fractional part is 0.5 or greater.
   - Example:

     ```basic
     VAR rounded = round(3.6)  # rounded will be 4
     ```

3. **not(value):**
   - The `not()` function is used to negate a boolean value. It changes a `true` value to `false` and a `false` value to `true`.
   - Example:

     ```basic
     VAR inverted = not(true)  # inverted will be false
     ```

Note: In the examples, values can be replaced by variables, constants, device variables, etc.

It's worth noting that there are more functions and operations to be developed in the BASIC programming language to further enhance its capabilities. As the language evolves, additional functions and features may become available.

## Custom Functions

In the BASIC programming language, you can create custom functions to perform specific operations or calculations. Custom functions are user-defined and allow you to encapsulate a block of code that can be reused throughout your program. Here's how to create custom functions:

1. **Define a Function:**
   - To create a custom function, you need to define it. You start by using the `FUNCTION` keyword followed by the function name and a list of parameters (up to 5 parameters).
   - Example of defining a custom function:

     ```basic
     FUNCTION addNumbers(value1, value2)
     ```
   - The variables used as parameters of the function must be unique in the code. Ideally, they should not be used in other functions (this allows the compiler to optimize the vaiables scope).
   - The function can be also declared without any parameter as `FUNCTION myFunction()`.

2. **Function Body:**
   - After defining the function, you provide the code block that makes up the function's body. This is where you specify what the function should do when it's called.
   - Example of the function body:

     ```basic
     VAR result = value1 + value2
     ```
   - Note that the function body can use its own variables (declared by the definition) and any other variables or device defining in the program.

3. **Return Value:**
   - If your function should return a value, you use the `RETURN` keyword followed by the value you want to return.
   - Example of returning a value from the function:

     ```basic
     RETURN result
     ```
   - The return value is optional. If no `RETURN` instruction is provided, the function will return `true`.

4. **End the Function:**
   - To finish defining the function, you use the `ENDFUNCTION` keyword.
   - Example:

     ```basic
     ENDFUNCTION
     ```

Here's the complete example of a custom function that adds two numbers:

```basic
FUNCTION addNumbers(value1, value2)
    VAR result = value1 + value2
    RETURN result
ENDFUNCTION
```

Once you've defined a custom function, you can call it elsewhere in your code by using the function name and passing arguments to it. For example:

```basic
VAR sum = addNumbers(3, 5)  # Calls the custom function and assigns the result to "sum"
```

Note that custom functions cannot be called in other custom functions (We have removed this possibility on purpose, as it require more stack operations while compiled).
Custom functions must be used for more complex operations that needs to be repeated in code. If you don't need to pass parameters to a function, or don't need a return value, prioritize the use of labels  with `goto` instructions.

# Contacts and suggestions

We value your feedback and suggestions! If you have any questions, ideas, or simply want to connect with the community, we encourage you to join our Discord server. It's a great platform to interact with like-minded individuals, ask questions, share your experiences, and provide valuable feedback. Your input helps us improve and enhance the resources and tools we provide. We look forward to having you as part of our community.

- Join us on Discord: https://discord.gg/2rnpHTWVyV