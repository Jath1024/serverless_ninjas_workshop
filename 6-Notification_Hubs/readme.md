Overview[](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification#overview)
------------------------------------------------------------------------------------------------------------------------------------------------------

This article shows you how to use Azure Notification Hubs to send push notifications to a Universal Windows Platform (UWP) app.

In this article, you create a blank Windows Store app that receives push notifications by using the Windows Push Notification Service (WNS). When you're finished, you can use your notification hub to broadcast push notifications to all devices that are running your app.

Before you begin[](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification#before-you-begin)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

The goal of this article is to help you get started using Azure Notification Hubs as quickly as possible. The article presents a very simple broadcast scenario that focuses on the basic concepts of Notification Hubs.

If you are already familiar with Notification Hubs, you might want to select another topic from the left navigation or go to the relevant articles in the "Next steps" section.

We take your feedback seriously. If you have any difficulty completing this topic, or if you have recommendations for improving this content, we invite you to provide feedback at the end of the article.

You can find the completed code for this tutorial on [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

Prerequisites[](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification#prerequisites)
----------------------------------------------------------------------------------------------------------------------------------------------------------------

This tutorial requires the following:

-   [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) or later
-   [UWP app-development tools installed](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
-   An active Azure account\
    If you don't have an account, you can create a free trial account in just a couple of minutes. For more information, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
-   An active Windows Store account

Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for UWP apps.

Register your app for the Windows Store[](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification#register-your-app-for-the-windows-store)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

To send push notifications to UWP apps, associate your app to the Windows Store. Then, configure your notification hub to integrate with WNS.

1.  If you have not already registered your app, navigate to the [Windows Dev Center](https://dev.windows.com/overview), sign in with your Microsoft account, and then select Create a new app.

2.  Type a name for your app, and then select Reserve app name. Doing so creates a new Windows Store registration for your app.

3.  In Visual Studio, create a new Visual C# Store apps project by using the UWP Blank App template, and then select OK.

4.  Accept the defaults for the target and minimum platform versions.

5.  In Solution Explorer, right-click the Windows Store app project, select Store, and then select Associate App with the Store.\
    The Associate Your App with the Windows Store wizard appears.

6.  In the wizard, sign in with your Microsoft account.

7.  Select the app that you registered in step 2, select Next, and then select Associate. Doing so adds the required Windows Store registration information to the application manifest.

8.  Back on the [Windows Dev Center](http://dev.windows.com/overview) page for your new app, select Services, select Push notifications, and then select WNS/MPNS.

9.  Select New Notification.

10. Select Blank (Toast) template, and then select OK.

11. Enter a notification Name and Visual Context message, and then select Save as draft.

12. Go to the [Application Registration Portal](http://apps.dev.microsoft.com/) and sign in.

13. Select your application name. In Windows Store platform settings, note the Application Secret password and the Package security identifier (SID).

    Warning

    The application secret and package SID are important security credentials. Do not share these values with anyone or distribute them with your app.

Configure your notification hub[](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification#configure-your-notification-hub)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1.  Sign in to the [Azure portal](https://portal.azure.com/).

2.  Select Create a resource > Web + Mobile > Notification Hub.

    ![Azure portal - create a notification hub](https://docs.microsoft.com/en-us/azure/includes/media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)

3.  In the Notification Hub box, type a unique name. Select your Region, Subscription, and Resource Group (if you have one already).

    If you don't already have a service bus namespace, you can use the default name, which is created based on the hub name (if the namespace name is available).

    If you already have a service bus namespace that you want to create the hub in, follow these steps

    a. In the Namespace area, select the Select Existing link.

    b. Select Create.

    ![Azure portal - set notification hub properties](https://docs.microsoft.com/en-us/azure/includes/media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4.  After you've created the namespace and notification hub, open it by selecting All resources and then select the created notification hub from the list.

    ![Azure portal - notification hub portal page](https://docs.microsoft.com/en-us/azure/includes/media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-resources.png)

5.  Select Access Policies from the list. Note the two connection strings that are available to you. You need them to handle push notifications later.

    Important

    Do NOT use the DefaultFullSharedAccessSignature in your application. This is meant to be used in your back-end only.

    ![Azure portal - notification hub connection strings](https://docs.microsoft.com/en-us/azure/includes/media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

1.  Under Notification Services, select Windows (WNS), and then enter the application secret password in the Security Key box. In the Package SID box, enter the value that you obtained from WNS in the previous section, and then select Save.

![The Package SID and Security Key boxes](https://docs.microsoft.com/en-us/azure/notification-hubs/media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Your notification hub is now configured to work with WNS. You have the connection strings to register your app and send notifications.

Connect your app to the notification hub[](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification#connect-your-app-to-the-notification-hub)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1.  In Visual Studio, right-click the solution, and then select Manage NuGet Packages.\
    The Manage NuGet Packages window opens.

2.  In the search box, enter Microsoft.Azure.NotificationHubs, select Install, and accept the terms of use.

    ![The Manage NuGet Packages window](https://docs.microsoft.com/en-us/azure/notification-hubs/media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png)

    This action downloads, installs, and adds a reference to the Azure Notification Hubs library for Windows by using the [Microsoft.Azure.NotificationHubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs).

3.  Open the App.xaml.cs project file, and add the following `using` statements:

    Copy

    ```
     using Windows.Networking.PushNotifications;
     using Microsoft.WindowsAzure.Messaging;
     using Windows.UI.Popups;

    ```

4.  In App.xaml.cs, also add to the App class the following InitNotificationsAsync method definition:

    Copy

    ```
     private async void InitNotificationsAsync()
     {
         var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

         var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
         var result = await hub.RegisterNativeAsync(channel.Uri);

         // Displays the registration ID so you know it was successful
         if (result.RegistrationId != null)
         {
             var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
             dialog.Commands.Add(new UICommand("OK"));
             await dialog.ShowAsync();
         }

     }

    ```

    This code retrieves the channel URI for the app from WNS, and then registers that channel URI with your notification hub.

    Note

    -   Replace the hub name placeholder with the name of the notification hub that appears in the Azure portal.
    -   Also replace the connection string placeholder with the DefaultListenSharedAccessSignature connection string that you obtained from the Access Polices page of your notification hub in a previous section.

5.  At the top of the OnLaunched event handler in App.xaml.cs, add the following call to the new InitNotificationsAsync method:

    Copy

    ```
     InitNotificationsAsync();

    ```

    This action guarantees that the channel URI is registered in your notification hub each time the application is launched.

6.  To run the app, select the F5 key. A dialog box that contains the registration key is displayed.

Your app is now ready to receive toast notifications.

Send notifications[](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification#send-notifications)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

You can quickly test receiving notifications in your app by sending notifications in the [Azure portal](https://portal.azure.com/). Use the Test Send button on the notification hub, as shown in the following image:

![The Test Send pane](https://docs.microsoft.com/en-us/azure/notification-hubs/media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)