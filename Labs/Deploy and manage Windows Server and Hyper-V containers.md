
# Exercise 1 — Task 1: Install Docker on Windows Server

- [ ] Sign in to **LON-SVR1** as **Contoso\Administrator** with the password:

    ```powershell
    Pa55w.rd
    ```

- [ ] Select **Start**, then open **Windows PowerShell ISE**.

- [ ] In the script pane of PowerShell ISE, enter the following commands on the first two lines:

    ```powershell
    Invoke-WebRequest -UseBasicParsing "https://raw.githubusercontent.com/microsoft/Windows-Containers/Main/helpful_tools/Install-DockerCE/install-docker-ce.ps1" -o install-docker-ce.ps1
    .\install-docker-ce.ps1
    ```

- [ ] From the **ISE toolbar**, select **Run Script**.

- [ ] Wait for Windows Server to **restart automatically**.

- [ ] After restart, sign in again as **Contoso\Administrator** with:

    ```powershell
    Pa55w.rd
    ```

- [ ] Allow the installation to continue until the message:

    ```
    Script complete!
    ```

- [ ] Close **PowerShell ISE**.

- [ ] If you receive an error message, **rerun the two-line script** from earlier.


### Exercise 1 — Task 2: Download an image

- [ ] On **LON-SVR1**, select **Start**, then open **Windows PowerShell ISE**.

- [ ] Close the **Untitled1.ps1** script pane without saving changes.

- [ ] At the PowerShell ISE command prompt, verify Docker by running:

    ```powershell
    docker container run hello-world:nanoserver
    ```

- [ ] Wait for the image to be pulled from the online repository and the container to run.

- [ ] Confirm you see output similar to:

    ```
    Hello from Docker!
    ```

- [ ] To view Docker images currently stored locally, run:

    ```powershell
    docker images
    ```

- [ ] Verify that the **hello-world:nanoserver** image appears in the list.

- [ ] To review available Microsoft base images in the online repository, run:

    ```powershell
    docker search Microsoft
    ```

- [ ] If the list does not appear the first time, run the **docker search** command again.

- [ ] To download a Windows Server Core IIS image that matches the host OS, run:

    ```powershell
    docker pull mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2022
    ```

- [ ] Wait for the image download to complete.  
      (This may take **15 minutes or more** depending on connection speed.)


### Exercise 2 — Task 1: Deploy a new container

- [ ] On **LON-SVR1**, open **Windows PowerShell ISE**.

- [ ] At the PowerShell command prompt, confirm the images that are currently downloaded:

    ```powershell
    docker images
    ```

- [ ] Verify that both **hello-world** and **IIS** images appear in the list.

- [ ] Run the IIS container in the background by entering:

    ```powershell
    docker run -d -p 8080:80 --name ContosoSite mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2022 cmd
    ```

- [ ] Retrieve the container host’s IP configuration by entering:

    ```powershell
    ipconfig
    ```

- [ ] Make note of the **IPv4 address** for:

    - The Ethernet adapter named **vEthernet (nat)** → This is the **container network**.
    - The Ethernet adapter named **Ethernet** → This is the **host (LON-SVR1)**.

- [ ] On **LON-SVR1**, open **Microsoft Edge**.

- [ ] In the address bar, enter:

    ```
    http://<HostIPAddress>:8080
    ```

    (Example: `http://172.16.0.21:8080`)

- [ ] Verify that the default **IIS webpage** loads, served from inside the Docker container.

### Exercise 2 — Task 2: Manage the container

- [ ] At the **Windows PowerShell ISE** command prompt on **LON-SVR1**, list running containers:

    ```powershell
    docker ps
    ```

- [ ] Note the container named **ContosoSite** in the output.

- [ ] Stop the running IIS container by entering:

    ```powershell
    docker stop ContosoSite
    ```

- [ ] Confirm that the container has stopped by running:

    ```powershell
    docker ps
    ```

- [ ] Verify that **ContosoSite** no longer appears in the list of running containers.