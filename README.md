# WPF

## Resources in WPF
Resources are values stored in a dictionary, Generally we provide a key and get back some sort of objects.

### Resources can be declared as

### 1.Windows Resources

        <Window.Resources>
                <SolidColorBrush Color="Red" x:Key="RedSolidBrush"></SolidColorBrush>
        </Window.Resources>
        
### 2.Control resources

        <Button Content="Click Me!!" Height="60" Width="160">
                 <Button.Resources>
                      <Style TargetType="Button">
                         <Setter Property="Background" Value="Red" />
                      </Style>
                 </Button.Resources>
        </Button>
        
### 3. Application resources

      <Application x:Class="WpfAppDemo.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:WpfAppDemo"
             StartupUri="MainWindow.xaml">
        <Application.Resources>
            <SolidColorBrush Color="Red" x:Key="RedSolidBrush"></SolidColorBrush>
        </Application.Resources>
      </Application>
      
In WPF there are two types of resources
### 1.Static Resource
### 2.Dynamic Resources

### Static Resource 
it is resolved at compile time, i.e. value of the object is set when you are working with XAML.

      <Window.Resources>
              <SolidColorBrush Color="Red" x:Key="RedSolidBrush"></SolidColorBrush>
      </Window.Resources>

      <Button Content="Click Me!!" Background="{StaticResource RedSolidBrush}" Height="60" Width="160"></Button>


### Dynamic Resource 
It is resolved at run time. The value of object is evaluated and stored in an expression and the value is substituted at run time. Use for localization of application.

      <StackPanel Orientation="Horizontal">
        <Button Content="Button" Background="{ DynamicResource ResourceKey=ChangeButtonColor} " Height="68" Width="118" />
        <Button Content="Red" Height="40" Width="93" Click="Button_Click"/>
        <Button Content="Blue" Height="40" Width="93" Click="Button_Click_1"/>
       </StackPanel>



       private void Button_Click(object sender, RoutedEventArgs e)
       {
         this.Resources["ChangeButtonColor"] = new SolidColorBrush { Color = Colors.Red };
       }

       private void Button_Click_1(object sender, RoutedEventArgs e)
       {
          this.Resources["ChangeButtonColor"] = new SolidColorBrush { Color = Colors.Blue };
       } 

### Style Inheritance

Use BasedOn attributes to use inheritance.

      <Application.Resources>
              <Style x:Key="btnWithLargeFont" TargetType="Button">
                  <Style.Setters>
                      <Setter Property="FontSize" Value="30"></Setter>
                  </Style.Setters>
              </Style>

              <Style x:Key="btnWithBoldFont" TargetType="Button" BasedOn="{StaticResource btnWithLargeFont}">
                  <Style.Setters>
                      <Setter Property="FontWeight" Value="Bold"></Setter>
                  </Style.Setters>
              </Style>
      </Application.Resources>

### Merging Dictionary 

Set Theme at run time

        <ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                            xmlns:local="clr-namespace:ResourceDemo">
            <Style x:Key="ButtonStyle" TargetType="Button">
                <Style.Setters>
                    <Setter Property="Background" Value="Blue"></Setter>
                </Style.Setters>
            </Style>
        </ResourceDictionary>

        <ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                            xmlns:local="clr-namespace:ResourceDemo">
            <Style x:Key="ButtonStyle" TargetType="Button">
                <Style.Setters>
                    <Setter Property="Background" Value="Red"></Setter>
                </Style.Setters>
            </Style>
        </ResourceDictionary>


        <Window x:Class="ResourceDemo.MainWindow"
                xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
                xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
                xmlns:local="clr-namespace:ResourceDemo"
                mc:Ignorable="d"
                Title="MainWindow" Height="450" Width="800">
            <Grid>
                <StackPanel>
                    <Button x:Name="btn1" Content="Make Red" Click="Button_Click"> </Button>
                    <Button x:Name="btn2" Content="Make Blue" Click="Button_Click_1"></Button>
                </StackPanel>
            </Grid>
        </Window>

        namespace ResourceDemo
        {
            /// <summary>
            /// Interaction logic for MainWindow.xaml
            /// </summary>
            public partial class MainWindow : Window
            {
                ResourceDictionary dc =null;
                public MainWindow()
                {
                    InitializeComponent();
                }

                private void Button_Click(object sender, RoutedEventArgs e)
                {
                    SetStyle("/red.xaml");
                }

                private void Button_Click_1(object sender, RoutedEventArgs e)
                {
                    SetStyle("/blue.xaml");
                }

                public void SetStyle(string name)
                {
                    if (!string.IsNullOrEmpty(name))
                    {
                        dc = new ResourceDictionary();

                        dc.Source = new Uri(name, UriKind.Relative);

                        btn1.Style = (Style)(dc["ButtonStyle"]);
                        btn2.Style = (Style)(dc["ButtonStyle"]);

                    }
                }
            }
        }
