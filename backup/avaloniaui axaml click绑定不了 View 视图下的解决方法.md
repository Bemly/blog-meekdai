## 开端

https://docs.avaloniaui.net/zh-Hans/docs/get-started/test-drive/respond-to-an-event

按照阿瓦隆UI框架的教程走，按照MVVM的思想处理，用VB在这里卡了半天，编译报错：

```xml
<!-- Views/MainWindow.axaml -->
<Window ... xmlns:vm="using:demo1.ViewModels"
        x:Class="demo1.Views.MainWindow"
        x:DataType="vm:MainWindowViewModel">
    <Button Click="Aaaa"></Button>
</Window>
```
```vb.net
' Views/MainWindow.axaml.vb
Imports Avalonia.Controls

Namespace Views
    Partial Public Class MainWindow
        Inherits Window

        ...
        
        Public Sub Aaaa()
            Debug.WriteLine("MainWindow")
        End Sub
    End Class
End Namespace
```

```
Unable to find suitable setter or adder for property Click of type Avalonia ... String
```

大致就是把`Aaaa`认作字符串了，我就纳闷了，是不是 VB 的解析库没有写好XML解析这个步骤

可以用`Command="{Binding }"`强行给我放到`x:DataType`里去，但是我们已经规定了`vm`为`ViewModels`，

不遵守规范写出来的特难看

```vb.net
' ViewModels/MainWindowViewModel.vb
Namespace ViewModels
    Partial Public Class MainWindowViewModel
        Inherits ViewModelBase

        Public ReadOnly Property Greeting As String
            Get
                Return "Welcome to Avalonia!"
            End Get
        End Property
        
        Public Sub Bbbb()
            Debug.WriteLine("MainWindow")
        End Sub
    End Class
End Namespace
```
```xml
<!-- Views/MainWindow.axaml -->
<Window ... xmlns:vm="using:demo1.ViewModels"
        x:Class="demo1.Views.MainWindow"
        x:DataType="vm:MainWindowViewModel">
    <StackPanel>
        <TextBlock Text="{Binding Greeting}" HorizontalAlignment="Center" VerticalAlignment="Center"/>
        <Button Command="Bbbb()"></Button>
    </StackPanel>
</Window>
```

## 解决办法

搞了半天，原来是我脑子短路了，绑定路由事件需要`Public`，\
在`Sub`或`Function`的参数里面调用`(source As Object, args As RoutedEventArgs)`，\
引入`Imports Avalonia.Interactivity`就能被正常解析了 草

```vb.net
' Views/MainWindow.axaml.vb
Imports Avalonia.Controls
Imports Avalonia.Interactivity

Namespace Views
    Partial Public Class MainWindow
        Inherits Window

        ...
        
        Public Sub Aaaa(source As Object, args As RoutedEventArgs)
            Debug.WriteLine("MainWindow")
        End Sub
    End Class
End Namespace
```
```xml
<!-- Views/MainWindow.axaml -->
<Window ... xmlns:vm="using:demo1.ViewModels"
        x:Class="demo1.Views.MainWindow"
        x:DataType="vm:MainWindowViewModel">
    <Button Click="Aaaa"></Button>
</Window>
```

希望官方能出个 VB 模板（，还好国内有个 VB 大佬给我们提供了 VB 代码转换器：\
https://github.com/Nukepayload2/Nukepayload2.SourceGenerators.AvaloniaUI

尝尝鲜：VB.NET跨平台开发是我做梦都没想到的

## 引用/致谢

Avalonia/issues/3898#issuecomment-625401692