# WPF

### User32 and GDI/GDI+

GDI/GDI+ has provided drawing support to the Windows OS for rendering as images, shapes, and text. These technologies are known to be relatively complicated to use and they provide poor performance. The User32 subsystem has provided the common look and feel for Windows
elements such as buttons, text boxes, windows, etc.

###  DirectX

DirectX was an effort to provide a toolkit to allow developers to create video games that could bypass the limitations of the GDI/User32 subsystems. The DirectX API was extremely complex and therefore it was not ideal for creating business application user interfaces.

### WPF

WPF is the most comprehensive graphical technology to build Business application for windows. WPF uses DirectX behind the scenes to render graphical content, allowing WPF to take advantage of hardware acceleration. This means that your applications will use
your GPU (your graphics card’s processor) as much as possible when rendering your WPF applications.

### User32 Limitations
WPF uses User32 in a limited capacity to handle the routing of your input controls (your mouse
and keyboard, for instance). However, all drawing functions have been passed on through
DirectX to provide monumental improvements in performance.

### User32 in a limited capacity

WPF uses User32 in a limited capacity to handle the routing of your input controls (your mouse and keyboard, for instance). However, all drawing functions have been passed on through DirectX to provide monumental improvements in performance.

### WPF and Windows 7/Windows Vista
WPF will perform best under Windows 7 or Windows Vista. This is because these operating systems allow the technology to take advantage of the Windows Display Driver Model (WDDM). WDDM allows scheduling of multiple GPU operations at the same time. It also provides a
mechanism to page video card memory with normal system memory when the video card memory threshold is exceeded. One of the first jobs of the WPF infrastructure is to evaluate your video card and provide a score or rating called a tier value. WPF recognizes three distinct tier values. The tier value descriptions follow, as provided by the Microsoft Developer Network documentation on WPF, available at https://msdn.microsoft.com/en-us/library/ms742196(v=vs.100).aspx:

### Rendering Tier 0: No graphics hardware acceleration. All graphics features use software
acceleration. The DirectX version level is lower than version 9.0.

### Rendering Tier 1: Some graphics features use graphics hardware acceleration. The DirectX
version level is higher than or equal to version 9.0.

### Rendering Tier 2: Most graphics features use graphics hardware acceleration. The DirectX
version level is higher than or equal to version 9.0.

## Layout controls

1. Grid

Methods for Sizing Grid Columns and Rows

Fixed - Fixed size of logical units (1/96 inch).
Auto-  Takes as much space as needed by the contained control.
Star (*) -
Takes as much space as available (after filling all auto and fixed sized columns), proportionally divided over all star-sized columns. So 3*/5* means the same as 30*/50*. Remember that star sizing does not work if the grid size is calculated based on its content.

2. Stack Panel

        <StackPanel>
                 <TextBlock Margin="10" FontSize="20">How do you like your coffee?</TextBlock>
                 <Button Margin="10">Black</Button>
                 <Button Margin="10">With milk</Button>
                 <Button Margin="10">Latte macchiato</Button>
                 <Button Margin="10">Cappuccino</Button>
         </StackPanel>
         
 OR
 
        <StackPanel Orientation="Horizontal" Height="49" Width="247">
                 <Button Width="100" Margin="10">OK</Button>
                 <Button Width="100" Margin="10">Cancel</Button>
         </StackPanel>

3. Dock Panel

        <DockPanel LastChildFill="True">
                 <Button Content="Dock=Top" DockPanel.Dock="Top"/>
                 <Button Content="Dock=Bottom" DockPanel.Dock="Bottom"/>
                 <Button Content="Dock=Left"/>
                 <Button Content="Dock=Right" DockPanel.Dock="Right"/>
                 <Button Content="LastChildFill=True"/>
        </DockPanel>

4. Wrap Panel

        <WrapPanel Orientation="Horizontal">
                 <Button Content="Button" />
                 <Button Content="Button" />
                 <Button Content="Button" />
                 <Button Content="Button" />
                 <Button Content="Button" />
        </WrapPanel>

5. Canvas

        <Canvas>
                 <Rectangle Canvas.Left="40" Canvas.Top="31" Width="63" Height="41" Fill="Blue"/>
                 <Ellipse Canvas.Left="130" Canvas.Top="79" Width="58" Height="58" Fill="Blue" />
                 <Path Canvas.Left="61" Canvas.Top="28" Width="133" Height="98" Fill="Blue"
                 Stretch="Fill" Data="M61,125 L193,28"/>
        </Canvas>
        
 OR
 
         <Canvas>
                 <Ellipse Fill="Green" Width="60" Height="60" Canvas.Left="30" Canvas.Top="20" Canvas.ZIndex="1"/>
                 <Ellipse Fill="Blue" Width="60" Height="60" Canvas.Left="60" Canvas.Top="40"/>
        </Canvas>


## Type Converters

The System.ComponentModel.TypeConverter class provides a unified way of converting XAML string attribute values to corresponding object value types. 

        <Button x:Name="btnDisplayButton" Grid.Column="1" Grid.Row="0" Background="Blue" 
        
        <Button x:Name="btnHexRGBColor" Grid.Column="1" Grid.Row="1" Background="#299" Content="Button" Width="100" Height="40" />

 We've specified the Background attribute on each of the button controls. In the first button, we simply specified the string name of
the color we wanted to use as the background. For the second button we specified the RGB values as a three digit hex value. The System.Drawing.ColorConverter class is responsible for providing this functionality.

### Implementing a TypeConverter
A class provides a unified way of converting XAML string attribute values to corresponding object value types. 

### CanConvertTo()
A support method that returns a Boolean indicating whether the value can be converted to the specified type.

### CanConvertFrom()
A support method that returns a Boolean indicating whether the value can be converted from a specified type.

### ConvertTo() 
Converts the given value object to the specified type.

### ConvertFrom() 
Converts the given value to the type of this converter.

You must apply the TypeConverterAttribute to the class that implements TypeConverter.Here is an example of the use of the attribute [TypeConverter(typeof(MyCustomConverter))].

                public class StringListTypeConverter : TypeConverter
                {
                    public override bool CanConvertFrom(ITypeDescriptorContext context, 
                        Type sourceType)
                    {
                      return sourceType == typeof(string);
                    }

                    public override object ConvertFrom(ITypeDescriptorContext context,
                        System.Globalization.CultureInfo culture, object value)
                    {
                      return new StringList((string)value);
                    }

                    public override bool CanConvertTo(ITypeDescriptorContext context, 
                        Type destinationType)
                    {
                      return destinationType == typeof(string);
                    }

                    public override object ConvertTo(ITypeDescriptorContext context,
                        System.Globalization.CultureInfo culture, object value, Type destinationType)
                    {
                      return value == null ? null : string.Join(", ", (StringList)value);
                    }
                }


                [TypeConverter(typeof(StringListTypeConverter))]
                class StringList : IEnumerable<string>
                {
                    private readonly IEnumerable<string> _enumerable;
                    private readonly string _original;

                    public StringList(string value)
                    {
                      _original = value;
                      if (!string.IsNullOrEmpty(value))
                      {
                        _enumerable = value.Split(",;".ToCharArray()).
                          Where(i => !string.IsNullOrWhiteSpace(i)).Select(i => i.Trim());
                      }
                    }

                    protected StringList(IEnumerable<string> value)
                    {
                      _enumerable = value;
                      _original = string.Join(", ", value);
                    }

                    public StringList() { }

                    public IEnumerator<string> GetEnumerator()
                    {
                      return _enumerable.GetEnumerator();
                    }

                    IEnumerator IEnumerable.GetEnumerator()
                    {
                      return _enumerable.GetEnumerator();
                    }

                    public override string ToString()
                    {
                      return _original;
                    }
                }


        <Window x:Class="TypeConverterExample.MainWindow" xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:TypeConverterExample" Title="TypeConverter Binding Example" Height="250" Width="375">
          <Window.DataContext>
            <local:ViewModel />
          </Window.DataContext>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto" />
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <TextBox Grid.Row="0" Text="{Binding Source}"/>
            <ListBox Grid.Row="1" ItemsSource="{Binding Source, Mode=TwoWay}"/>
          </Grid>
        </Window>

## Using the TypeConverter Directly

        TypeConverter converter = TypeDescriptor.GetConverter(targetType);
        string convertedValue = converter.ConvertFrom(value);


## Resources in WPF
Resources are values stored in a dictionary, Generally we provide a key and get back some sort of objects.
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
        
## Routed Events
### 1.Direct Event
### 2.Tunneling Event
### 3.Bubbling Event 

## Dependency Properties
WPF has provided some extended services to the CLR property that we can collectively call Dependency Properties. A Dependency Property is a property whose value depends on the external sources, such as animation, data binding, styles, or visual tree inheritance. Not only this, but a Dependency Property also has the built-in feature of providing notification when the property has changed, data binding and styling.

### Key features
1.Style
2.Callbacks
3.Animation
4.Resource
5.Data Binding
Etc

### Less memory consumption
The Dependency Property stores the property only when it is altered or modified. Hence a huge amount of memory for fields are free.

### Property value inheritance
It means that if no value is set for the property then it will return to the inheritance tree up to where it gets the value.

### Change notification and Data Bindings
Whenever a property changes its value it provides notification in the Dependency Property using INotifyPropertyChange and also helps in data binding.

### Participation in animation, styles and templates
A Dependency Property can animate, set styles using style setters and even provide templates for the control.

### CallBacks
Whenever a property is changed you can have a callback invoked.

### Resources
You can define a Resource for the definition of a Dependency Property in XAML.

### Overriding Metadata
You can define certain behavior of a Dependency Property using PropertyMetaData. Thus, overriding a meta data from a derived property will not require you to redefine or re-implement the entire property definition.

        public class CarDependencyProperty : DependencyObject
         {
                //Register Dependency Property
                public static readonly DependencyProperty CarDependency = DependencyProperty.Register("MyProperty", typeof(string),                         typeof(MainWindow));
                public string MyCarProperty
                {
                    get
                    {
                        return (string)GetValue(CarDependency);
                    }
                    set
                    {
                        SetValue(CarDependency, value);
                    }
                }
        }
        
          <Window x:Class="CustomDependencyProperties.MainWindow"
                xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
                xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
                xmlns:local="clr-namespace:CustomDependencyProperties"
                mc:Ignorable="d"
                Title="MainWindow" Height="450" Width="800">
            <Window.Resources>
                <local:CarDependencyProperty x:Key="CarDependency" />
            </Window.Resources> 
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition />
                    <RowDefinition />
                </Grid.RowDefinitions>
                <Label Content="Enter Car:" Grid.Row="0"
                   VerticalAlignment="Center" />
                <TextBox Text="{Binding Path=MyCarProperty, Source={StaticResource CarDependency }}" Name="MyTextCar" Height="25" 			Width="150" />
                <Button Name="MyButton" Content="Click Me!" Click="MyButton_Click" Height="25" Width="150" Grid.Row="1" />
            </Grid>
        </Window>
        
         public partial class MainWindow : Window
         {
                public MainWindow()
                {
                    InitializeComponent();
                }
                private void MyButton_Click(object sender, RoutedEventArgs e)
                {
                    CarDependencyProperty dpSample = TryFindResource("CarDependency") as CarDependencyProperty;
                    MessageBox.Show(dpSample.MyCarProperty);
                }
          }

## Attached Properties

These properties attached to the child control by the parent controls, these are not property of child control.

Defining class must provide GerPropertyName set SetPropertyName method. In the case of grid
Grid class may have GetRow() and SetRow()

## Triggers
### 1.Property Triggers

                <Button Content="Click me to change style" Width="200" Height="30">
                                <Button.Style>
                                    <Style TargetType="Button">
                                        <Setter Property="Foreground" Value="Blue"></Setter>
                                        <Style.Triggers>
                                            <Trigger Property="IsMouseOver" Value="true">
                                                <Trigger.Setters>
                                                    <Setter Property="Foreground" Value="Red"></Setter>
                                                    <Setter Property="FontWeight" Value="Bold"></Setter>
                                                </Trigger.Setters>
                                            </Trigger>
                                        </Style.Triggers>
                                    </Style>
                                </Button.Style>
                 </Button>

### 2.Data Triggers

                <CheckBox x:Name="ChkBx"></CheckBox>
                 <TextBlock>
                     <TextBlock.Style>
                         <Style TargetType="TextBlock">
                             <Setter Property="Text" Value="I am here!"></Setter>
                             <Setter Property="Foreground" Value="Red"></Setter>
                             <Setter Property="FontWeight" Value="Bold"></Setter>
                             <Style.Triggers>
                              <DataTrigger Binding="{Binding ElementName=ChkBx, Path=IsChecked}" Value="True">
                                     <Setter Property="Text" Value="I am not here!"></Setter>
                                     <Setter Property="Foreground" Value="Blue"></Setter>
                                     <Setter Property="FontWeight" Value="Bold"></Setter>
                              </DataTrigger>
                             </Style.Triggers>
                         </Style>
                     </TextBlock.Style>
                 </TextBlock>

### 3.Event Triggers

                <TextBlock Text="Hello India">
                 <TextBlock.Style>
                     <Style TargetType="TextBlock">
                         <Style.Triggers>
                             <EventTrigger RoutedEvent="MouseEnter">
                                 <EventTrigger.Actions>
                                     <BeginStoryboard>
                                       <Storyboard>
                                       <DoubleAnimation Duration="0:0:0.300" Storyboard.TargetProperty="FontSize" To="28" />
                                       </Storyboard>
                                      </BeginStoryboard>
                                   </EventTrigger.Actions>
                              </EventTrigger>
                              <EventTrigger RoutedEvent="MouseLeave">
                                    <EventTrigger.Actions>
                                      <BeginStoryboard>
                                          <Storyboard>
                                       <DoubleAnimation Duration="0:0:0.300" Storyboard.TargetProperty="FontSize" To="15">                                                      </DoubleAnimation>
                                          </Storyboard>
                                     </BeginStoryboard>
                                   </EventTrigger.Actions>
                               </EventTrigger>
                            </Style.Triggers>
                          </Style>
                        </TextBlock.Style>
                     </TextBlock>

## Binding Modes
### 1.OneWay
### 2.TwoWay
### 3.OneWayToSource
### 4.OneTime
### 5.Default

## UpdateSourceTrigger
### 1.Default
### 2.Explicit
### 3.LostFocus
### 4.PropertyChanges

## IValueConverter

                class YesNoToBooleanConverter : IValueConverter
                {
                    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
                    {
                        if (value is string && !string.IsNullOrEmpty(value.ToString()))
                        {
                            switch (value.ToString().ToLower())
                            {
                                case "yes":
                                    return true;
                                case "no":
                                    return false;

                            }
                        }

                        return false;
                    }

                 public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
                        {
                            if (value is bool)
                            {
                                if ((bool)value == true)
                                    return "yes";
                                else
                                    return "no";
                            }
                            return "no";
                        }
                }


                <Window x:Class="WpfAppDemo.WindowDynamicResource"
                        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
                        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
                        mc:Ignorable="d"
                        xmlns:local="clr-namespace:WpfAppDemo"
                        Title="WindowDynamicResource" Height="450" Width="800">
                    <Window.Resources>
                        <local:YesNoToBooleanConverter x:Key="YesNoToBoolean"></local:YesNoToBooleanConverter>
                    </Window.Resources>
                    <Grid Margin="50">
                        <StackPanel Orientation="Vertical">

                            <TextBox x:Name="txtValue" Height="30" Width="100"></TextBox>
                            <TextBlock Text="{Binding ElementName=txtValue, Path=Text, Converter={StaticResource YesNoToBoolean}}">                                 </TextBlock>

                        </StackPanel>
                    </Grid>
                </Window>

## INotifyPropertyChanged

           class Person : INotifyPropertyChanged
            {
                public Person()
                {
                    this.FirstName = "Jcob";
                    this.LastName = "Marsh";
                }

                public event PropertyChangedEventHandler PropertyChanged;

                public void OnPropertyChange(string propertyName)
                {
                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
                    }
                }


                private string Fname;
                public string FirstName
                {
                    get { return Fname; }
                    set
                    {
                        Fname = value;
                        OnPropertyChange("FirstName");
                        OnPropertyChange("FullName");
                    }
                }


                private string Lname;
                public string LastName
                {
                    get { return Lname; }
                    set
                    {
                        Lname = value;
                        OnPropertyChange("FirstName");
                        OnPropertyChange("FullName");
                    }
                }

                private string FuName;

                public string FullName
                {
                    get { return FirstName+" "+LastName; }
                    set
                    {
                        FuName = value;
                        OnPropertyChange("FullName");
                    }
                }
        }


        <Window x:Class="INotifyPropertyChangedDemo.MainWindow"
                xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
                xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
                xmlns:local="clr-namespace:INotifyPropertyChangedDemo"
                mc:Ignorable="d"
                Title="MainWindow" Height="450" Width="800">
            <Window.Resources>
                <local:Person x:Key="Person"></local:Person>
            </Window.Resources>
            <Grid DataContext="{StaticResource Person}">

                <StackPanel>
                    <TextBox x:Name="FirstName" Text="{Binding Path=FirstName, Mode=TwoWay}"></TextBox>
                    <TextBox x:Name="LastName" Text="{Binding Path=LastName, Mode=TwoWay}"></TextBox>
                    <TextBlock Text="{Binding Path=FullName}"></TextBlock>
                </StackPanel>

            </Grid>
        </Window>

## ICommand

            class Person : INotifyPropertyChanged
            {
                public Person()
                {
                    this.FirstName = "Jcob";
                    this.LastName = "Marsh";
                }

                public event PropertyChangedEventHandler PropertyChanged;

                public void OnPropertyChange(string propertyName)
                {
                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
                    }
                }


                private string Fname;
                public string FirstName
                {
                    get { return Fname; }
                    set
                    {
                        Fname = value;
                        OnPropertyChange("FirstName");
                        OnPropertyChange("FullName");
                    }
                }


                private string Lname;
                public string LastName
                {
                    get { return Lname; }
                    set
                    {
                        Lname = value;
                        OnPropertyChange("FirstName");
                        OnPropertyChange("FullName");
                    }
                }

                private string FuName;

                public string FullName
                {
                    get { return FirstName+" "+LastName; }
                    set
                    {
                        FuName = value;
                        OnPropertyChange("FullName");
                    }
                }




            }


            public class ViewModelBase
            {
                public SimpleCommand SimpleCommand { get; set; }

                public ViewModelBase()
                {
                    this.SimpleCommand = new SimpleCommand(this);
                }

                public void SimpleMethod()
                {
                    MessageBox.Show("Simple Method()");
                }
            }


            public class SimpleCommand : ICommand
            {
                public event EventHandler CanExecuteChanged;

                public ViewModelBase ViewBaseModel { get; set; }
                public SimpleCommand(ViewModelBase viewModelBase)
                {
                    this.ViewBaseModel = viewModelBase;
                }

                public bool CanExecute(object parameter)
                {
                    return true;
                }

                public void Execute(object parameter)
                {
                    this.ViewBaseModel.SimpleMethod();
                }
          }


         xmlns:vm="clr-namespace:INotifyPropertyChangedDemo.ViewModel"


        <Button Content="Simple Command" Command="{Binding SimpleCommand, Source={StaticResource viewModel}}"></Button>


        <Window.Resources>
                <vm:ViewModelBase x:Key="viewModel"></vm:ViewModelBase>
        </Window.Resources>
