## Exercise 5: Create a function that integrates with other services

### Task 1: Upload sample content to Azure Blob Storage

1. On the Azure portal's **navigation** pane, select the **Resource groups** link.
1. On the **Resource groups** blade, select the **Serverless** resource group that you created previously in this lab.
1. On the **Serverless** blade, select the **funcstor**_[yourname]_ storage account that you created previously in this lab.
1. On the **Storage account** blade, select the **Containers** link in the **Data storage** section.
1. In the **Containers** section, select **+ Container**.
1. In the **New container** pop-up window, perform the following actions, and then select **Create**:

    | Setting | Action |
    | -- | -- |
    | **Name** text box  | Enter **content** |
    | **Public access level** drop-down list  | Select **Private (no anonymous access)** |

1. Return to the **Containers** section, and then select the recently created **content** container.
1. On the **Container** blade, select **Upload**.
1. In the **Upload blob** window, perform the following actions, and then select **Upload**:

    | Setting | Action |
    | -- | -- |
    | **Files** section  | Select the **Folder** icon |
    | **File Explorer** window  | Browse to **Allfiles (F):\\Allfiles\\Labs\\02\\Starter**, select the **settings.json** file, and then select **Open** |
    | **Overwrite if files already exist** check box | Ensure that this check box is selected |

      > **Note**: Wait for the blob to upload before you continue with this lab.

### Task 2: Create an HTTP-triggered function

1. On the taskbar, select the **Windows Terminal** icon.
1. Run the following command to change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func** directory:

    ```powershell
    cd F:\Allfiles\Labs\02\Starter\func
    ```

1. From the command prompt, run the following command to use the **Azure Functions Core Tools** to create a new function named **GetSettingInfo**, using the **HTTP trigger** template:

    ```powershell
    func new --template "HTTP trigger" --name "GetSettingInfo"
    ```

    > **Note**: You can review the documentation to [create a new function][azure-functions-core-tools-new-function] using the **Azure Functions Core Tools**.
1. Close the currently running **Windows Terminal** application.

### Task 3: Write HTTP-triggered and blob-inputted function code

1. On the **Start** screen, select the **Visual Studio Code** tile.
1. On the **File** menu, select **Open Folder**.
1. In the **File Explorer** window that opens, browse to **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func**, and then select **Select Folder**.
1. On the **Explorer** pane of the **Visual Studio Code** window, open the **GetSettingInfo.cs** file.
1. In the code editor, observe the example implementation:

    ```csharp
    using System;
    using System.IO;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Extensions.Http;
    using Microsoft.AspNetCore.Http;
    using Microsoft.Extensions.Logging;
    using Newtonsoft.Json;    
    namespace func
    {
        public static class GetSettingInfo
        {
            [FunctionName("GetSettingInfo")]
            public static async Task<IActionResult> Run(
                [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req,
                ILogger log)
            {
                log.LogInformation("C# HTTP trigger function processed a request.");    
                string name = req.Query["name"];    
                string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
                dynamic data = JsonConvert.DeserializeObject(requestBody);
                name = name ?? data?.name;    
                string responseMessage = string.IsNullOrEmpty(name)
                    ? "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response."
                    : $"Hello, {name}. This HTTP triggered function executed successfully.";    
                return new OkObjectResult(responseMessage);
            }
        }
    }
    ```

1. Delete all the content within the **GetSettingInfo.cs** file.

1. Add the following lines of code to add **using directives** for the **Microsoft.AspNetCore.Http**, **Microsoft.AspNetCore.Mvc**, and **Microsoft.Azure.WebJobs** namespaces:

    ```csharp
    using Microsoft.AspNetCore.Http;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.WebJobs;
    ```

1. Create a new **public static** class named **GetSettingInfo**:

    ```csharp
    public static class GetSettingInfo
    { }
    ```

1. Observe the **GetSettingInfo.cs** file again, which should now include the following code:

    ```csharp
    using Microsoft.AspNetCore.Http;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.WebJobs;
    public static class GetSettingInfo
    { }
    ```

1. Within the **GetSettingInfo** class, add the following code block to create a new **public static** expression-bodied method named **Run** that returns a variable of type **IActionResult** and that also takes in variables of type **HttpRequest** and **string** as parameters named *request* and *json*:

    ```csharp
    public static IActionResult Run(
        HttpRequest request,
        string json)
        => null;
    ```

    > **Note**: You are only temporarily setting the return value to **null**.

1. Add the following code to append an attribute to the **Run** method of type **FunctionNameAttribute** that has its **name** parameter set to a value of **GetSettingInfo**:

    ```csharp
    [FunctionName("GetSettingInfo")]
    public static IActionResult Run(
        HttpRequest request,
        string json)
        => null;
    ```

1. Add the following code to append an attribute to the **request** parameter of type **HttpTriggerAttribute** that has its **methods** parameter array set to a single value of **GET**:

    ```csharp
    [FunctionName("GetSettingInfo")]
    public static IActionResult Run(
        [HttpTrigger("GET")] HttpRequest request,
        string json)
        => null;
    ```

1. Add the following code to append an attribute to the **json** parameter of type **BlobAttribute** that has its **blobPath** parameter set to a value of **content/settings.json**:

    ```csharp
    [FunctionName("GetSettingInfo")]
    public static IActionResult Run(
        [HttpTrigger("GET")] HttpRequest request,
        [Blob("content/settings.json")] string json)
        => null;
    ```

1. Add the following code to update the **Run** expression-bodied method to return a new instance of the **OkObjectResult** class passing in the value of the **json** method parameter as the sole constructor parameter:

    ```csharp
    [FunctionName("GetSettingInfo")]
    public static IActionResult Run(
        [HttpTrigger("GET")] HttpRequest request,
        [Blob("content/settings.json")] string json)
        => new OkObjectResult(json);
    ```

1. Observe the **GetSettingInfo.cs** file again, which should now include the following code:

    ```csharp
    using Microsoft.AspNetCore.Http;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.WebJobs;
    public static class GetSettingInfo
    {
        [FunctionName("GetSettingInfo")]
        public static IActionResult Run(
            [HttpTrigger("GET")] HttpRequest request,
            [Blob("content/settings.json")] string json)
            => new OkObjectResult(json);
    }
    ```

1. Select **Save** to save your changes to the **GetSettingInfo.cs** file.

### Task 4: Register Azure Storage Blob extensions

1. On the taskbar, select the **Windows Terminal** icon.
1. Run the following command to change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func** directory:

    ```powershell
    cd F:\Allfiles\Labs\02\Starter\func
    ```

1. From the command prompt, run the following command to register the [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) extension:

    ```powershell
    func extensions install --package Microsoft.Azure.WebJobs.Extensions.Storage --version 5.0.1
    ```

1. Run the following command to build the .NET project and to validate the extensions were installed correctly:

    ```powershell
    dotnet build
    ```

1. Close all currently running instances of the **Windows Terminal** application.

### Task 5: Test the function by using httprepl

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
1. On the taskbar, select the **Windows Terminal** icon again to open a new instance of the **Windows Terminal** application.
1. From the command prompt, run the following command to start the **httprepl** tool setting the base Uniform Resource Identifier (URI) to ``http://localhost:7071``:

    ```powershell
    httprepl http://localhost:7071
    ```

    > **Note**: An error message is displayed by the **httprepl** tool. This message occurs because the tool is searching for a Swagger definition file to use to traverse the API. Because your function project doesn't produce a Swagger definition file, you'll need to traverse the API manually.

1. When you receive the tool prompt, run the following command to browse to the relative **api** endpoint:

    ```powershell
    cd api
    ```

1. Run the following command to browse to the relative **getsettinginfo** endpoint:

    ```powershell
    cd getsettinginfo
    ```

1. Run the following command to run the **get** command for the current endpoint:

    ```powershell
    get
    ```

1. Observe the JSON content of the response from the function app, which should now include:

    ```json
    {
        "version": "0.2.4",
        "root": "/usr/libexec/mews_principal/",
        "device": {
            "id": "21e46d2b2b926cba031a23c6919"
        },
        "notifications": {
            "email": "joseph.price@contoso.com",
            "phone": "(425) 555-0162 x4151"
        }
    }
    ```

1. Run the following command to exit the **httprepl** application:

    ```powershell
    exit
    ```

1. Close all currently running instances of the **Windows Terminal** application.

## Review

In this exercise, you created a function that returns the content of a JSON file in Storage.
