## Lab 6: Deploy a local function project to an Azure Functions app

### Task 1: Deploy using the Azure Functions Core Tools

1. On the taskbar, select the **Windows Terminal** icon.
1. Run the following command to change the current directory to the **Allfiles (F):\\Allfiles\\Labs\\02\\Starter\\func** directory:

    ```powershell
    cd F:\Allfiles\Labs\02\Starter\func
    ```

1. From command prompt, run the following command to login to the Azure Command-Line Interface (CLI):

    ```powershell
    az login
    ```

1. In the **Microsoft Edge** browser window, enter the name and password of the Microsoft or Azure Active Directory account you are using in this lab, and then select **Sign in**.
1. Return to the currently open **Windows Terminal** window. Wait for the sign-in process to finish.
1. From the command prompt, run the following command to publish the function app project (replace the `<function-app-name>` placeholder with the name of the function app you created earlier in this lab):

    ```powershell
    func azure functionapp publish <function-app-name>
    ```

    > **Note**: For example, if your **Function App name** is **funclogicstudent**, your command would be ``func azure functionapp publish funclogicstudent``. You can review the documentation to [publish the local function app project][azure-functions-core-tools-publish-azure] using the **Azure Functions Core Tools**.

1. Wait for the deployment to finalize before you move forward with the lab.
1. Close the currently running **Windows Terminal** application.

### Task 2: Validate deployment

1. On the taskbar, select the **Microsoft Edge** icon, and select the tab that displays the Azure portal (<https://portal.azure.com>).
1. On the Azure portal's **navigation** pane, select the **Resource groups** link.
1. On the **Resource groups** blade, select the **Serverless** resource group that you created previously in this lab.
1. On the **Serverless** blade, select the **funclogic**_[yourname]_ function app that you created previously in this lab.
1. On the **Function App** blade, select the **Functions** option in the **Functions** section.
1. On the **Functions** pane, select the existing **GetSettingInfo** function.
1. In the **Function** blade, select the **Code + Test** option in the **Developer** section.
1. In the function editor, select **Test/Run**.
1. In the automatically displayed pane, in the **HTTP method** drop-down list, select **GET**.
1. Select **Run** to test the function.
1. In the **HTTP response content**, review the results of the test run. The JSON content should now include the following code:

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

## Review

In this exercise, you deployed a local function project to Azure Functions and validated that the functions work in Azure.
