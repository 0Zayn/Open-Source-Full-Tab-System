# [Open-Source] Blaze - Full Tab System
Full tab system containing all the information you need to start up your new application. This project has now been released, and will be open for all to use and learn from, feel free to use this in your application, and please do not distribute this under your name. I do not want people to use this for materials they will release for paid use, so please do not. Also this is a tutorial for begginers, so dont be lazy and ask for Source Code, I didn't write all this just for people to skip it and go straight to installation without learning anything.

# Setup

To start first create a new folder, in your application and name it ```bin```, next inside that create a folder named ```tabs```
Congratulations, you should now have this: 

![Required Files](https://github.com/BlazeCreates/-Open-Source-Blaze---Full-Tab-System/raw/main/image%20(1).png)

Now you need a Monaco/Ace Editor I would use Monaco, here is the download for a basic one, I do not have time to teach you how to make one from scratch so enjoy!

[Download Monaco Editor](https://github.com/BlazeCreates/-Open-Source-Blaze---Full-Tab-System/raw/main/Monaco.zip)

Next you will need to unzip this file, using 7Zip or WinRar, I will not explain how to do this, you should know by now this is a daily task.

[For Stupid People](https://www.youtube.com/watch?v=Tpkk3NKk-Tw&ab_channel=Aspiration)

After you unzipped it (hoping you did not use that link) you will need to Cut or Copy this folder, and place it in your application folder, which is either Debug OR Release navigate to your application directory to do this. It should now be fully implemented, so congrats.

To use this you either need CefSharp or WebView2 so I will teach you how to install CefSharp

### How to install CefSharp

So first navigate to your **Project** pop up on the top of your Visual Studio and select **Manage NuGet Packages...**

![Navigation](https://media.discordapp.net/attachments/1081737726048608379/1086674219267539004/image.png?width=520&height=631)

Make sure you in the **Browse** tab and search for ```CefSharp.Wpf``` for a WPF Application or if your using WinForms ```CefSharp.WinForms```

I am using a WPF application so these instructions will all be for WPF, not WinForms but you can easily figure out how to do this for WinForms

Now once you've found it make sure it's by **The CefSharp Authors** and install the latest stable version

After installation make sure to add this code in your Window Element in the **MainWindow.xaml** The code to add:

```xmlns:cef="clr-namespace:CefSharp.Wpf;assembly=CefSharp.Wpf"```

Now you are succesfully done and ready to move on to the rest of the steps
