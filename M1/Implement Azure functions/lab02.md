## Exercise 2: Configure a local Azure Functions project

### Task 1: Initialize a function project

1. On the taskbar, select the **Windows Terminal** icon.

1. Run the following command to change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func** empty directory:

    ```powershell
    cd F:\Allfiles\Labs\02\Starter\func
    ```

    > **Note**: In Windows Explorer remove the **Read-only** attribute from **F:\\Allfiles\\Labs\\02\\Starter\\func\\.gitignore** file.

1. Run the following command to use the **Azure Functions Core Tools** to create a new local Azure Functions project in the current directory using the **dotnet** runtime:

    ```powershell
    func init --worker-runtime dotnet --force
    ```

    > **Note**: You can review the documentation to [create a new project][azure-functions-core-tools-new-project] using the **Azure Functions Core Tools**.
    
1. Close the **Windows Terminal** application.

### Task 2: Configure a connection string

1. On the **Start** screen, select the **Visual Studio Code** tile.
1. On the **File** menu, select **Open Folder**.
1. In the **File Explorer** window that opens, browse to **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func**, and then select **Select Folder**.
1. On the **Explorer** pane of the **Visual Studio Code** window, open the **local.settings.json** file.
1. Observe the current value of the **AzureWebJobsStorage** setting:

    ```json
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    ```

1. Change the value of the **AzureWebJobsStorage** element to the **connection string** of the storage account that you recorded earlier in this lab.
1. Save the **local.settings.json** file.

### Task 3: Build and validate a project

1. On the taskbar, select the **Windows Terminal** icon.
1. Run the following command to change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func** directory:

    ```powershell
    cd F:\Allfiles\Labs\02\Starter\func
    ```

1. Run the following command to **build** the .NET Core 3.1 project:

    ```powershell
    dotnet build
    ```

## Review

In this exercise, you created a local project that you'll use for Azure Functions development.
