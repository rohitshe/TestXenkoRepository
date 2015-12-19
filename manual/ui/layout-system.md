# Layout System

In Xenko, Layout System is very similar to the one in WPF.

You can find a much more detailed explanation in [WPF Layout System documentation](http://msdn.microsoft.com/en-us/library/ms745058%28v=vs.100%29.aspx) .

# Introduction

Every `UIElement (ref:{SiliconStudio.Xenko.UI.UIElement})` in UI System has a surrounding rectangle area that is used while layouting.

Layouting will be computed when necessary according to the requirements of the UIElement, available screen space, constraints, margin, padding, and the special layout behavior of `Panel (ref:{SiliconStudio.Xenko.UI.Controls.Panel})` elements (which arrange children in their own specific ways).

Processing this data recursively, the layout system will be able to compute a position and size for every `UIElement (ref:{SiliconStudio.Xenko.UI.Controls.UIElement})` in the UI System.

# Measure and Arrange

Layout is performed recursively in two passes, Measure and Arrange.

## Measure

During the Measure pass, starting with a call `Measure (ref:{SiliconStudio.Xenko.UI.UIElement.Measure})`, every element will recursively compute its `DesiredSize (ref:{SiliconStudio.Xenko.UI.UIElement.DesiredSize})` according to user-given properties such as `Width (ref:{SiliconStudio.Xenko.UI.UIElement.Width})`, `Height (ref:{SiliconStudio.Xenko.UI.UIElement.Height})`, `Margin (ref:{SiliconStudio.Xenko.UI.UIElement.Margin})` and `Padding (ref:{SiliconStudio.Xenko.UI.UIElement.Padding})`.

Some `Panel (ref:{SiliconStudio.Xenko.UI.Controls.Panel})` element might call `Measure (ref:{SiliconStudio.Xenko.UI.UIElement.Measure})` recursively to determine `DesiredSize (ref:{SiliconStudio.Xenko.UI.UIElement.DesiredSize})` size of their children and act accordingly.

## Arrange

The Arrange pass, starting with a call to `Arrange (ref:{SiliconStudio.Xenko.UI.UIElement.Arrange})`, will actually arrange the element with the actual size available. Taking into account `Margin (ref:{SiliconStudio.Xenko.UI.UIElement.Margin})`, `Padding (ref:{SiliconStudio.Xenko.UI.UIElement.Padding})`, `Width (ref:{SiliconStudio.Xenko.UI.UIElement.Width})` and `Height (ref:{SiliconStudio.Xenko.UI.UIElement.Height})`, as well as `HorizontalAlignment (ref:{SiliconStudio.Xenko.UI.UIElement.HorizontalAlignment})`, `VerticalAlignment (ref:{SiliconStudio.Xenko.UI.UIElement.VerticalAlignment})` and some new `Panel (ref:{SiliconStudio.Xenko.UI.Controls.Panel})` specific `Arrange (ref:{SiliconStudio.Xenko.UI.UIElement.Arrange})` rules, final element size and position will be computed.

 

 

