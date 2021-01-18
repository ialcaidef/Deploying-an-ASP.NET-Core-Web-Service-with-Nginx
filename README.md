# Deploying-an-ASP.NET-Core-Web-Service-with-Nginx
20487_DEMO06_L2


Deploying an ASP.NET Core Web Service with Nginx

Demonstration Steps

1. Open Microsoft Edge.
2. Navigate to https://portal.azure.com.
3. On the left menu panel, click Virtual machines.
4. On Virtual Machines blade, click Create virtual machine. On next page make sure it selects the latest version of Ubuntu Server in image column.
5. To create a new virtual machine, in the Basics view, provide the following details:
   - In the Virtual Machine Name box, type myvm.
   - Click Disks and from the OS disk type list select Standard HDD.
   - In the Username box, type myadmin.
   - In the Authentication type list, select Password, and then  in the Password and Confirm Password boxes, type Password123!.
   - In the Resource group section click Create new and type Mod6Demo2ResourceGroup and then click OK.
   - Click OK.
6. In the Size section click change size and provide the following details:
   - Select D2s_v3.
   - Click Select.
7. In the Inbound Port Rules section , select Allow selected ports and provide the following details:
   - In Select public inbound ports, select the following: HTTP(80), HTTPS(443), and SSH(22).
   - Click OK.
   
   ![20487D_Images](https://github.com/ialcaidef/Deploying-an-ASP.NET-Core-Web-Service-with-Nginx/blob/master/Images/01.png)
   
8. In the Summary view, click Review + Create and then click Create.
9. Open the command prompt.
10. At the command prompt, to change the directory to Demo2Project, run the following command:
        cd [Repository Root]\Allfiles\Mod06\Demofiles\Demo2Project
11. To open the project in Microsoft Visual Studio Code, run the following command:
        code .
12. In Demo2Project - Visual Studio Code, on the left menu, double-click Demo2Project.csproj.
13. In the Demo2Project.csproj file, locate the <PropertyGroup> element, and inside it, paste the following code:
        <RuntimeIdentifiers>win10-x64;osx.10.11-x64;ubuntu.16.10-x64</RuntimeIdentifiers>
14. Switch to the command prompt.
15. At the command prompt, to publish the app as Self-contained, run the following command:
        dotnet publish -c release -r ubuntu.16.10-x64
16. Switch to Azure Portal.
17. Click Virtual machines, and then select myvm.
18. In the top panel, click Connect and copy the Login using VM local account value.
19. Switch to the command prompt.
20. Paste the Login using VM local account value and press Enter.
21. At the command prompt, enter yes.
22. Type the password Password123!.
23. To switch the directory to var folder, paste the following command:
        cd /var
24. To create a new folder named demo, paste the following command:
        sudo mkdir demo
25. To change ownership of the directory, paste the following command:
        sudo chown myadmin demo
26. Open a new command prompt instance.
27. To change the directory to the published folder, paste the following command:
        cd [Repository Root]\Allfiles\Mod06\Demofiles\Demo2Project\bin\release\netcoreapp2.1
28. To publish the package to the virtual machine, paste the following command:
        scp -r .\ubuntu.16.10-x64\ myadmin@<server_IP_address>:/var/demo
    Note: Change <server_IP_address> with the IP address of your virtual machine.
29. Type the following password Password123! and press Enter.
30. Switch to the command prompt that connected to the virtual machine.
31. To switch the directory to the Root folder, run the following command:
        cd /
32. To install Nginx, run the following commands:
        sudo -s
        nginx=stable
        add-apt-repository ppa:nginx/$nginx
        apt-get update
        apt-get install nginx
33. When finished, press Enter again.
34. When the question: Do you want to continue? appears, type y, and then press Enter.
35. To start the Nginx service, run the following command:
        sudo service nginx start
36. Verify that the browser displays the default landing page for Nginx. The landing page is:
        http://<server_IP_address>/index.nginx-debian.html
37. To change the directory to the Nginx configuration folder, run the following command:
        cd /etc/nginx/sites-available/
38. To open the configuration file in the Vim text editor, run the following command:
        vi default
39. To change to edit mode, press ESC + I.
40. Replace the Location content with the following code:
            proxy_pass         http://localhost:5000;
            proxy_http_version 1.1;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection keep-alive;
            proxy_set_header   Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Proto $scheme;

![20487D_Images](https://github.com/ialcaidef/Deploying-an-ASP.NET-Core-Web-Service-with-Nginx/blob/master/Images/02.png)
   
41. To save the file and exit, press ESC + : + x + Enter.
42. To verify that the syntax of the configuration file is correct, run the following command:
        sudo nginx -t
43. To force Nginx to pick up the the changes, run the following command:
        sudo nginx -s reload
44. To switch the directory to the publish folder, run the following command:
        cd /var/demo/ubuntu.16.10-x64/publish/
45. To execute the premissions to binary, run the following command:
        chmod a+x ./Demo2Project
46. To run the app, run the following command:
        ./Demo2Project
47. Open Microsoft Edge.
48. Navigate to the following URL:
        <server_IP_address>/api/values
        
![20487D_Images](https://github.com/ialcaidef/Deploying-an-ASP.NET-Core-Web-Service-with-Nginx/blob/master/Images/03.png)
        
49. Verify the response is:
        ["value1","value2"]
        
![20487D_Images](https://github.com/ialcaidef/Deploying-an-ASP.NET-Core-Web-Service-with-Nginx/blob/master/Images/04.png)

Lesson 3: 
