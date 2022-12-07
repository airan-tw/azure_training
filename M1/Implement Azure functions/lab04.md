## Exercise 4: Create a function that triggers on a schedule

### Task 1: Create a schedule-triggered function

1. On the taskbar, select the **Windows Terminal** icon.
1. Run the following command to change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func** directory:

    ```powershell
    cd F:\Allfiles\Labs\02\Starter\func
    ```

1. From the command prompt, run the following command to use the **Azure Functions Core Tools** to create a new function named **Recurring**, using the **Timer trigger** template:

    ```powershell
    func new --template "Timer trigger" --name "Recurring"
    ```

    > **Note**: You can review the documentation to [create a new function][azure-functions-core-tools-new-function] using the **Azure Functions Core Tools**.
    
1. Close the currently running **Windows Terminal** application.

### Task 2: Observe function code

1. On the **Start** screen, select the **Visual Studio Code** tile.
1. On the **File** menu, select **Open Folder**.
1. In the **File Explorer** window that opens, browse to **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func**, and then select **Select Folder**.
1. On the **Explorer** pane of the **Visual Studio Code** window, open the **Recurring.cs** file.
1. In the code editor, observe the implementation:

    ```csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.Extensions.Logging;    
    namespace func
    {
        public static class Recurring
        {
            [FunctionName("Recurring")]
            public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
            {
                log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
            }
        }
    }
    ```

### Task 3: Observe function runs

1. On the taskbar, select the **Windows Terminal** icon.
1. Run the following command to change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func** directory:

    ```powershell
    cd F:\Allfiles\Labs\02\Starter\func
    ```

1. From the command prompt, run the following command to run the function app project:

    ```powershell
    func start --build
    ```

    > **Note**: You can review the documentation to [start the function app project locally][azure-functions-core-tools-start-function] using the **Azure Functions Core Tools**.
1. Observe the function run that occurs about every five minutes. Each function run should render a simple message to the log.
1. Close the currently running **Windows Terminal** application.

### Task 4: Update the function integration configuration

1. On the **Start** screen, select the **Visual Studio Code** tile.
1. On the **File** menu, select **Open Folder**.
1. In the **File Explorer** window that opens, browse to **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func**, and then select **Select Folder**.
1. On the **Explorer** pane of the **Visual Studio Code** window, open the **Recurring.cs** file.
1. In the code editor, observe the existing **Run** method signature:

    ```csharp
    [FunctionName("Recurring")]
    public void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
    ```

1. Update the **Run** method signature code block to change the schedule to run once every **30 seconds**:

    ```csharp
    [FunctionName("Recurring")]
    public void Run([TimerTrigger("*/30 * * * * *")]TimerInfo myTimer, ILogger log)
    ```

1. Select **Save** to save your changes to the **Recurring.cs** file.

### Task 5: Observe function runs

1. On the taskbar, select the **Windows Terminal** icon.

1. Run the following command to change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\02\    \Starter\\func** directory:

    ```powershell
    cd F:\Allfiles\Labs\02\Starter\func
    ```

1. 1. From the command prompt, run the following command to run the function app project:

    ```powershell
    func start --build
    ```
    
    > **Note**: You can review the documentation to [start the function app project locally][azure-functions-core-tools-start-function] using the **Azure Functions Core Tools**.
    
1. Observe the function run that occurs about every 30 seconds. Each function run should render a simple message to the log.

1. Close the currently running **Windows Terminal** application.

1. Close the Visual Studio Code window.

## Review

In this exercise, you created a function that runs automatically based on a fixed schedule.
