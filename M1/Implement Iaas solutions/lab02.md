### Exercise 2: Create a Docker container image and deploy it to Azure Container Registry

#### Task 1: Open the Cloud Shell and editor

1.  On the Azure portal's navigation pane, select the **Cloud Shell** icon to open a new shell instance.  

    > **Note**: Wait for Cloud Shell to finish connecting to an instance before moving on with the lab.

1.  At the **Cloud Shell** command prompt in the portal, run the following command to move from the root directory to the **\~/clouddrive** directory:

    ```
    cd ~/clouddrive
    ```

1.  Run the following command to create a new directory named **ipcheck** in the **\~/clouddrive** directory:

    ```
    mkdir ipcheck
    ```

1.  Run the following command to change the active directory from **\~/clouddrive** to **\~/clouddrive/ipcheck**:

    ```
    cd ~/clouddrive/ipcheck
    ```

1.  Run the following command to create a new .NET console application in the current directory:

    ```
    dotnet new console --output . --name ipcheck --framework net6.0
    ```

1.  Run the following command to create a new file in the **\~/clouddrive/ipcheck** directory named **Dockerfile**:

    ```
    touch Dockerfile
    ```

1.  Run the following command to open the embedded graphical editor in the context of the current directory:

    ```
    code .
    ```

#### Task 2: Create and test a .NET application

1.  In the graphical editor, on the **FILES** pane, select the **Program.cs** file to open it in the editor.

1.  Delete the entire contents of the **Program.cs** file.

1.  Copy and paste the following code into the **Program.cs** file:

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {        
            // Check if network is available
            if (System.Net.NetworkInformation.NetworkInterface.GetIsNetworkAvailable())
            {
                System.Console.WriteLine("Current IP Addresses:");

                // Get host entry for current hostname
                string hostname = System.Net.Dns.GetHostName();
                System.Net.IPHostEntry host = System.Net.Dns.GetHostEntry(hostname);
                
                // Iterate over each IP address and render their values
                foreach(System.Net.IPAddress address in host.AddressList)
                {
                    System.Console.WriteLine($"\t{address}");
                }
            }
            else
            {
                System.Console.WriteLine("No Network Connection");
            }
        }
    }
    ```

1.  Save the **Program.cs** file by using the menu in the graphical editor or the Ctrl+S keyboard shortcut.  Don't close the graphical editor.

1.  Back at the command prompt, run the following command to run the application:

    ```
    dotnet run
    ```

1.  Review the results of the run. At least one IP address should be listed for the Cloud Shell instance.

1.  In the graphical editor, on the **FILES** pane of the editor, select the **Dockerfile** file to open it in the editor.

1.  Copy and paste the following code into the **Dockerfile** file:

    ```
    # Start using the .NET 6 SDK container image
    FROM mcr.microsoft.com/dotnet/sdk:6.0-alpine AS build

    # Change current working directory
    WORKDIR /app

    # Copy existing files from host machine
    COPY . ./

    # Publish application to the "out" folder
    RUN dotnet publish --configuration Release --output out

    # Start container by running application DLL
    ENTRYPOINT ["dotnet", "out/ipcheck.dll"]
    ```

1. Save the **Dockerfile** file by using the menu in the graphical editor or by using the Ctrl+S keyboard shortcut.

1. Leave the Cloud Shell open for the next task.

#### Task 3: Create a Container Registry resource

1. At the **Cloud Shell** command prompt in the portal, run the following command to create a variable with a unique value for the Container Registry resource: 

    ```bash
    registryName=conregistry$RANDOM
    ```

1. At the **Cloud Shell** command prompt in the portal, run the following command to verify the name created in the previous step is available: 

    ```bash
    az acr check-name --name $registryName
    ```

    If the results show the name is available, continue to the next step. If the name is not available then re-run the command in the previous step and verify availability again.

1. At the **Cloud Shell** command prompt in the portal, run the following command to create a Container Registry resource: 

    ```bash
    az acr create --resource-group ContainerCompute --name $registryName --sku Basic
    ```

    > **Note**: Wait for the creation task to complete before you continue with this lab.

#### Task 4: Store Container Registry metadata

1.  At the **Cloud Shell** command prompt in the portal, run the following command to get a list of all container registries in your subscription:

    ```
    az acr list
    ```

1.  Run the following command, ensuring you see the name of your registry as output. If you see no output other than '[]', wait a minute and try running the command again.

    ```
    az acr list --query "max_by([], &creationDate).name" --output tsv
    ```

1.  Run the following command:

    ```
    acrName=$(az acr list --query "max_by([], &creationDate).name" --output tsv)
    ```

1.  Run the following command:

    ```
    echo $acrName
    ```

#### Task 5: Deploy a Docker container image to Container Registry

1.  Run the following command to change the active directory from **\~/** to **\~/clouddrive/ipcheck**:

    ```
    cd ~/clouddrive/ipcheck
    ```

1.  Run the following command to get the contents of the current directory:

    ```
    dir
    ```

1.  Run the following command to upload the source code to your container registry and build the container image as a Container Registry task:

    ```
    az acr build --registry $acrName --image ipcheck:latest .
    ```

    > **Note**: Wait for the build task to complete before moving forward with this lab.

1.  Close the **Cloud Shell** pane in the portal.

#### Task 6: Validate your container image in Container Registry

1.  On the Azure portal's **navigation** pane, select the **Resource groups** link.

1.  From the **Resource groups** blade, select the **ContainerCompute** resource group that you created previously in this lab.

1.  From the **ContainerCompute** blade, select the container registry that you created previously in this lab.

1.  From the **Container Registry** blade, in the **Services** section, select the **Repositories** link.

1.  In the **Repositories** section, select the **ipcheck** container image repository, and then select the **latest** tag.

1.  Review the metadata for the version of your container image with the **latest** tag.

    > **Note**: You can also select the **Run ID** link to find metadata about the build task.

### Review

In this exercise, you created a .NET console application to display a machine’s current IP address. You then added the **Dockerfile** file to the application so that it could be converted into a Docker container image. Finally, you deployed the container image to Container Registry.
