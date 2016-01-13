# Layout System

In Xenko, Layout System is very similar to the one in WPF.

You can find a much more detailed explanation in [WPF Layout System documentation](http://msdn.microsoft.com/en-us/library/ms745058%28v=vs.100%29.aspx) .

# Introduction

Every @'SiliconStudio.Xenko.UI.UIElement' in UI System has a surrounding rectangle area that is used while layouting.

Layouting will be computed when necessary according to the requirements of the UIElement, available screen space, constraints, margin, padding, and the special layout behavior of @'SiliconStudio.Xenko.UI.Controls.Panel' elements (which arrange children in their own specific ways).

Processing this data recursively, the layout system will be able to compute a position and size for every @'SiliconStudio.Xenko.UI.Controls.UIElement' in the UI System.

# Measure and Arrange

Layout is performed recursively in two passes, Measure and Arrange.

## Measure

During the Measure pass, starting with a call @'SiliconStudio.Xenko.UI.UIElement.Measure', every element will recursively compute its @'SiliconStudio.Xenko.UI.UIElement.DesiredSize' according to user-given properties such as @'SiliconStudio.Xenko.UI.UIElement.Width', @'SiliconStudio.Xenko.UI.UIElement.Height', @'SiliconStudio.Xenko.UI.UIElement.Margin' and @'SiliconStudio.Xenko.UI.UIElement.Padding'.

Some @'SiliconStudio.Xenko.UI.Controls.Panel' element might call @'SiliconStudio.Xenko.UI.UIElement.Measure' recursively to determine @'SiliconStudio.Xenko.UI.UIElement.DesiredSize' size of their children and act accordingly.

## Arrange

The Arrange pass, starting with a call to @'SiliconStudio.Xenko.UI.UIElement.Arrange', will actually arrange the element with the actual size available. Taking into account @'SiliconStudio.Xenko.UI.UIElement.Margin', @'SiliconStudio.Xenko.UI.UIElement.Padding', @'SiliconStudio.Xenko.UI.UIElement.Width' and @'SiliconStudio.Xenko.UI.UIElement.Height', as well as @'SiliconStudio.Xenko.UI.UIElement.HorizontalAlignment', @'SiliconStudio.Xenko.UI.UIElement.VerticalAlignment' and some new @'SiliconStudio.Xenko.UI.Controls.Panel' specific @'SiliconStudio.Xenko.UI.UIElement.Arrange' rules, final element size and position will be computed.

 

 

