# [Open-Source] Blaze - Full Tab System
Full tab system containing all the information you need to start up your new application. This project has now been released, and will be open for all to use and learn from, feel free to use this in your application, and please do not distribute this under your name. I do not want people to use this for materials they will release for paid use, so please do not. Also this is a tutorial for begginers, so dont be lazy and ask for Download Links, I didn't write all this just for people to skip it and go straight to installation without learning anything.

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


# Code and Rest of Steps

Now you need to add a Tab Control you can do this by going to your Toolbox. How you would accomplish this is heading to the **View** Popup and clicking **Toolbox** OR you can use the shortcut ```CTRL + ALT + X``` and add in a Tab Control you can size it how you like and customize it to your liking

Now remove this extra tab from your tab control you only need one

```
<TabItem Header="TabItem">
  <Grid Background="#FFE5E5E5"/>
</TabItem>
```

Also remove this code snippet from the tab ```<Grid Background="#FFE5E5E5"/>``` and replace it with 

```<cef:ChromiumWebBrowser x:Name="MonacoEditor" Address="{Binding}"/>```

Name your TabControl to **EditorControl** you can do this by adding this line inside the element ```x:Name="EditorControl"``` 

Now your code should look like this:

```
<TabControl x:Name="EditorControl" HorizontalAlignment="Left" Height="420" Margin="0,0,-0.4,0" VerticalAlignment="Top" Width="794">
  <TabItem Header="Main Tab">
    <cef:ChromiumWebBrowser x:Name="MonacoEditor" Address="{Binding}"/>
  </TabItem>
</TabControl>
```

Now under your TabControl in the XAML code add this button to your code 
```<Button x:Name="AddTabButton" Content="+" HorizontalAlignment="Left" Margin="760,-1,0,0" VerticalAlignment="Top" Width="24" Height="24"/>```

Place it where ever you want and feel free to customize this, this will create tabs for you.

Now Click on the button and click on the lightning bolt and double click on **Click** to program it

![Programming Button](https://media.discordapp.net/attachments/1081737726048608379/1086687429349605446/image.png?width=551&height=622)

Now inside your C# code the the button contains, add this code all the lines are explained:

```
webBrowserCount += 1;

            // Create new tab item
            var tabItem = new TabItem();

            // You can change these to how you like it
            var closeButton = new Button
            {
                Content = "x",
                Width = 14,
                Height = 14,
                Foreground = Brushes.Black,
                Background = Brushes.Transparent,
                HorizontalAlignment = HorizontalAlignment.Center,
                VerticalAlignment = VerticalAlignment.Center,
                BorderThickness = new Thickness(0)
            };
            closeButton.Click += CloseTabButton_Click;

            // Set tab item header with close button
            tabItem.Header = new StackPanel
            {
                Orientation = Orientation.Horizontal,
                Children =
        {
            // You can also change these
            new TextBlock
            {
                Text = "NewTab" + webBrowserCount.ToString(),
                Margin = new Thickness(0,0,5,0),
                Foreground = Brushes.Black,
        },
            closeButton
        }
            };

            // Set the content of the tab item to a new Monaco editor
            var monacoEditor = new CefSharp.Wpf.ChromiumWebBrowser()
            {
                Name = "MonacoEditor" + webBrowserCount.ToString()
            };



            monacoEditor.Address = AppDomain.CurrentDomain.BaseDirectory + "/Monaco/index.html";
            monacoEditor.Name = "MonacoEditor" + webBrowserCount.ToString();
            tabItem.Content = monacoEditor;
            // Apply custom style to the tab item

            // Add tab item to tab control
            EditorControl.Items.Insert(EditorControl.Items.Count, tabItem);


            // Set the new tab as the selected tab
            EditorControl.SelectedItem = tabItem;
```

Make sure to add these under the button code outside the brackets

TabCount
```int webBrowserCount = 0;```

Close Button for Tabs
```
private void CloseTabButton_Click(object sender, RoutedEventArgs e)
        {
            var button = sender as Button;

            if (button != null)
            {
                var parentTabItem = FindVisualParent<TabItem>(button);

                if (parentTabItem != null)
                {
                    webBrowserCount -= 1;
                    EditorControl.Items.Remove(parentTabItem);
                }
            }
        }
```

Find Parents Task
```
private T FindVisualParent<T>(UIElement element) where T : UIElement
        {
            UIElement parent = element;

            while (parent != null)
            {
                T correctlyTyped = parent as T;

                if (correctlyTyped != null)
                {
                    return correctlyTyped;
                }

                parent = VisualTreeHelper.GetParent(parent) as UIElement;
            }

            return null;
        }
```

DO not forget to add these necessary usings:

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;
using CefSharp;
using CefSharp.Wpf;
```

Under ```InitializeComponent();``` Add this code to set the MainTabs Monaco Editor
```
// Get the project's folder path
            var ProjectPath = System.IO.Path.GetDirectoryName(System.Reflection.Assembly.GetExecutingAssembly().Location);

            var filePath = System.IO.Path.Combine(ProjectPath, "Monaco", "index.html");

            // Load the HTML file into the CefSharp browser control
            MonacoEditor.Load(filePath);
```

Now you can feel free to test this out make sure everything is working before we implement a saving system.

### Saving System

Now the part you have been waiting for, the tab saving system, we all want our important scripts to save after closing our application, so this is how we will accomplish this:

All these are described based on what the code is doing so feel free to learn from all these code snippets

Saving System
```
private void SaveTabs()
        {
            // Delete all existing tab files
            DirectoryInfo di = new DirectoryInfo(AppDomain.CurrentDomain.BaseDirectory + "/bin/tabs/");
            foreach (FileInfo file in di.GetFiles())
            {
                file.Delete();
            }

            // Loop through all open tabs and save the contents to a file
            foreach (TabItem tabItem in EditorControl.Items)
            {
                // Get the tab name
                string tabName = null;
                StackPanel stackPanel = tabItem.Header as StackPanel;
                if (stackPanel != null)
                {
                    TextBlock textBlock = stackPanel.Children.OfType<TextBlock>().FirstOrDefault();
                    if (textBlock != null)
                    {
                        tabName = textBlock.Text;
                    }
                }

                if (tabName == null)
                {
                    continue;
                }

                // Get the Monaco editor control
                var monacoEditor = tabItem.Content as CefSharp.Wpf.ChromiumWebBrowser;

                // Get the code from the Monaco editor
                var code = monacoEditor.EvaluateScriptAsync("editor.getValue()").Result.Result.ToString();

                // Write the code to a file
                File.WriteAllText(AppDomain.CurrentDomain.BaseDirectory + $"/bin/tabs/{tabName}.lua", code);
            }
        }
```

Loading Tabs (This took me a while, I had to fix so many errors)

```
private void LoadTabs()
        {
            // Get all the Lua files in the tabs directory
            DirectoryInfo di = new DirectoryInfo(AppDomain.CurrentDomain.BaseDirectory + "/bin/tabs/");
            FileInfo[] files = di.GetFiles("*.lua");

            // Loop through all the Lua files and create a new tab for each
            foreach (FileInfo file in files)
            {
                // Get the tab name from the file name
                string tabName = System.IO.Path.GetFileNameWithoutExtension(file.Name);

                // Create new tab item
                var tabItem = new TabItem();


                var closeButton = new Button
                {
                    Content = "x",
                    Width = 14,
                    Height = 14,
                    Foreground = Brushes.Black,
                    Background = Brushes.Transparent,
                    HorizontalAlignment = HorizontalAlignment.Center,
                    VerticalAlignment = VerticalAlignment.Center,
                    BorderThickness = new Thickness(0)
                };
                closeButton.Click += CloseTabButton_Click;

                // Set tab item header with close button
                tabItem.Header = new StackPanel
                {
                    Orientation = Orientation.Horizontal,
                    Children =
            {
                new TextBlock
                {
                    Text = tabName,
                    Margin = new Thickness(0,0,5,0),
                    Foreground = Brushes.Black,
                },
                closeButton
            }
                };

                // Set the content of the tab item to a new Monaco editor
                var monacoEditor = new CefSharp.Wpf.ChromiumWebBrowser()
                {
                    Name = "MonacoEditor" + webBrowserCount.ToString()
                };

                monacoEditor.Address = AppDomain.CurrentDomain.BaseDirectory + "/Monaco/index.html";
                monacoEditor.Name = "MonacoEditor" + webBrowserCount.ToString();

                // Set the tab item content to the Monaco editor
                tabItem.Content = monacoEditor;

                // Add tab item to tab control
                EditorControl.Items.Insert(EditorControl.Items.Count, tabItem);

                // Wait for frame to load and then set the value of the editor
                monacoEditor.FrameLoadEnd += (s, e) =>
                {
                    string code = File.ReadAllText(file.FullName);
                    monacoEditor.ExecuteScriptAsync("editor.setValue(`" + code.Replace("`", "\\`") + "`);");
                };

                webBrowserCount++;
            }
        }
```

These are all the external created tabs, I have no idea why I wrote it like this do not complain after all I worked pretty hard to get this to you guys

Saving Main Tab
```
private void SaveMainEditor()
        {
            // Get the content of the Monaco editor
            var script = "editor.getValue();";
            var code = ExecuteJavascriptOnMainEditor(script);

            // Save the code to the file
            string filePath = AppDomain.CurrentDomain.BaseDirectory + "/Main.lua";
            File.WriteAllText(filePath, code);
        }

        private string ExecuteJavascriptOnMainEditor(string script)
        {
            if (MonacoEditor == null)
            {
                return "";
            }

            var task = MonacoEditor.EvaluateScriptAsync(script);
            task.Wait();

            if (task.Result != null && task.Result.Success && task.Result.Result != null)
            {
                return task.Result.Result.ToString();
            }

            return "";
        }
```

Loading the Main Tab
```
private void LoadMainEditor()
        {
            string filePath = AppDomain.CurrentDomain.BaseDirectory + "/Main.lua";
            if (File.Exists(filePath))
            {
                string code = File.ReadAllText(filePath);

                if (MonacoEditor.IsBrowserInitialized)
                {
                    MonacoEditor.ExecuteScriptAsync("editor.setValue(`" + code.Replace("`", "\\`") + "`);");
                }
                else
                {
                    MonacoEditor.FrameLoadEnd += (sender, args) =>
                    {
                        MonacoEditor.ExecuteScriptAsync("editor.setValue(`" + code.Replace("`", "\\`") + "`);");
                    };
                }
            }
        }
```

### How to finish up and Implement saving

So after all this, we are finally almost done!
Navigate over to your Main Window select the Window Element in your viewport or your XAML code, and click the Lightning Bolt and find **Closing** and double click it

![Window Closing](https://media.discordapp.net/attachments/1081737726048608379/1086691192093278330/image.png?width=546&height=652)

Now Add in this code to Save the Editors Code inside the Window_Closing:
```
SaveTabs();
SaveMainEditor();
```

Finally add this to load the editors right between your editor loading and ```InitializeComponent();```

```
LoadTabs();
LoadMainEditor();
```

You are finally done for more info and help reach my discord at **Zayn#4479** 

Thank you, for reading all this and using my code!
