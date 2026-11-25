# Changelog

## 0.16.0 (2026-11-25)

- A floating toolbar can be resized from its right and bottom edges. The buttons wrap into a grid that fills the window, and the window snaps to whole cells so a row stays one height and there is no empty strip. The smallest size is one button plus the caption and the frame. A window at least as wide as it is tall is horizontal, including a square. A taller window is vertical, and the Vertical check follows while the edge is dragged. Checking or unchecking Vertical sizes the window to one column or one row, including a window that holds several toolbars. Escape or a lost capture keeps the size and orientation from the press. `SaveLayout` writes that width and height with the other float bounds, and loading the layout opens the window at that size. A float that was never resized stays a single row or column. Docked toolbars do not resize this way.
- The tray menu stays open while a toolbar is checked or unchecked, so several can be shown or hidden together. New Toolbar still closes it.
- Dragging a toolbar onto an empty window edge shows that edge's drop line in the guide overlay. The right and bottom lines sit inside the window, and dropping there creates the band. A point in that edge's zone docks on that edge even when it is also in the slop of a tray on another side. A point inside the tray still uses the tray.
- Docking a custom toolbar no longer reports a binding error for a copied button that is already its own container.
- A button copied onto another toolbar keeps the original button's width and height. It keeps the original style when that style's target type fits the copy, and uses the toolbar's button style otherwise. A check box uses the toggle style. A vertical float of copied icon buttons is the same size as the toolbar they came from.
- The close button on a vertical float caption is 16 DIP. A 24 DIP icon column keeps a drag area beside it. A column of 16 DIP or less does not. A horizontal caption stays at 22 DIP.

## 0.15.1 (2026-11-25)

- A vertical floating toolbar hides the caption grip and makes the caption outside the close button the drag handle, so the float is as wide as its button column or the close button (22 DIP) and a 24 DIP icon column leaves no empty strip.
- A toolbar's button band is the same height as its grip and its chevron, docked or floating.
- Dragging a toolbar off its tray shows the floating window on that move, under the pointer, while the button is still down.

## 0.15.0 (2026-11-25)

- Closing the docking guides now repaints a native pane the drop preview covered. Before, the preview could stay drawn on that pane.
- The bright edge follows the active pane. A selected tab or a lone caption keeps the panel fill. The 2px edge is `Dock.Accent` while that pane is active and `Dock.AccentInactive` otherwise. Docked groups, floating windows, and the auto-hide flyout use the same rule. `IsAccented` still means the one-pane top bar. `IsActive` is what a custom style reads for the bright edge. Dark, light, and blue each define `Dock.AccentInactive`. A `DockCustomTheme` dictionary may set `Dock.AccentInactive`, and a `Dock.*` key on the manager wins over that dictionary.
- A click or focus inside hosted native content, including a child of that window, activates the pane. The message stays with the native window, and focus is not moved.
- A floating toolbar window is as wide as its caption, buttons, and chevron. A vertical float with several buttons fits that column. An empty horizontal custom toolbar fits the grip and the chevron, and switching it to vertical and back measures it again. Two vertical toolbars in one window share that width.

## 0.14.0 (2026-11-24)

- Dragging a toolbar over an edge that has no band shows the drop line, and the edge guides when the pointer is over a pane or a floating window, and does not open a tray until you drop it there. Escape, or dropping somewhere else, leaves the edge empty. A band that is already there still takes the toolbar while the pointer is down. A new row in a tray waits for the drop as well, because that row would make the tray taller.
- Pulling a toolbar off its band shows the floating window on that move.
- Dragging a floating toolbar by its caption moves the window with the pointer on each move, the same way a pane float does.
- Dragging the grip of a floating toolbar that is alone in its window moves the window from the first move, too. A grip press right after a drag is no longer read as a double-click, so it does not dock the toolbar back.
- The New Toolbar and Rename dialog follows the dock theme. The window, caption, text, text box, and OK and Cancel buttons pick up `Theme`, a custom theme, and any `Dock.*` brushes set on the manager.
- Customize Toolbars is gone. The tray menu and Add or Remove Buttons no longer have that item, and a plain drag no longer moves or copies a button. Alt+drag still does. New Toolbar stays on the tray menu.
- A tray menu item no longer shows a tooltip that only repeats its label. That tooltip was drawn in the main window, away from the menu, in the system colors. A tooltip that says something else sits under the item and uses the dock tooltip colors.
- A vertical floating toolbar sizes to its buttons, including a custom toolbar that was horizontal first and a window that holds several toolbars. Switching back to horizontal sizes it again. The overflow chevron shows only when a button does not fit. Combined vertical toolbars share one width, with a separator between them.
- A custom toolbar's name is the caption and grip tooltip. It is not drawn in the caption, including a vertical float.

## 0.13.0 (2026-11-24)

- Floating toolbars can share one window. Drag a grip onto a float to insert it, drag inside the window to reorder, and drag out to tear one toolbar off. The caption moves the whole window, and dropping it on a tray docks every visible toolbar in order. Close and Alt+F4 hide the whole group and keep the float. A grip double-click or Reset still affects only that toolbar. The caption joins their titles. `MergeFloatingToolBars` and `SplitFloatingToolBar` do the same, and `floatId` and `floatIndex` are omitted when they are the default so an older file stays the same bytes.
- Closing a floating toolbar now hides it. Showing it again brings it back floating where it was. Reset still docks it on its declared place.
- A floating toolbar can stack its buttons. Check Vertical on the caption menu or on Add or Remove Buttons, or drag the right edge in past half the width or the bottom edge down past half the height. The window snaps to the new size. Escape or a lost capture keeps the orientation from the press. `DockManager.SetToolBarFloatVertical` and `IsToolBarFloatVertical` set and read it, and `SaveLayout` writes `floatVertical` only when it is true. Docking, docking back, and reset clear it. A float torn off a left or right edge stays horizontal until then. A docked toolbar ignores the setter, and a file that sets the flag on a toolbar that is not floating loads without it.
- You can make your own toolbars. New Toolbar on the tray menu asks for a name and floats an empty bar near the pointer. Alt+drag copies a button onto it and leaves the original in place. The copy clicks the original, and an Image or Path icon shows in both. Customize Toolbars turns that drag on without Alt until Escape or you leave the window. Rename and Delete are on the caption and the grip. Reset clears the copies and leaves the bar where it is. `CreateCustomToolBar`, `RenameCustomToolBar`, `DeleteCustomToolBar`, and `IsToolBarCustomizing` do the same, and `custom`, `title`, and `items` are omitted when they are the default so an older file stays the same bytes.
- Alt+drag or Customize Toolbars grabs a separator, or any thin item, from anywhere in its slot, not only on the painted line.
- A single click on the caption of a maximized floating window no longer restores it and moves it to the top-left corner. A click or a press without movement leaves it maximized, and a double-click restores it to its earlier place and size.
- Dragging the caption of a maximized floating window restores it to its earlier size under the pointer, at the same share of the caption width and the same height on it, and the drag continues, docking guides included. It used to shrink into the top-left corner.

## 0.12.0

- Dragging a toolbar so its trailing edge lands within 16 device-independent pixels of a band's far end, or past it, pins that toolbar to the end. Dragging it off that zone packs it from the start at the drop. The toolbar moves there while the pointer is still down.
- Reset on a toolbar's customize menu restores the edge, band, index, offset, and alignment it was declared with, docks it if it was floating, and shows it. Button changes still reset as before.
- A toolbar drag no longer crashes when the docking guide window is not shown yet or has already closed.

## 0.11.0

- `DockManager.CustomTheme` takes a `DockCustomTheme`. Subclass it and return the Uri of a resource dictionary. The manager reads `Dock.*` and `DockMenu.*` from that dictionary, including keys merged into it, and paints them over `Theme`. A key the dictionary does not define stays on the palette. A `Dock.*` key in `DockManager.Resources` still wins. Clearing `CustomTheme` restores the palette. A Uri that cannot be loaded throws when you set it.

## 0.10.0

First release on nuget.org. 0.9.0 was never published there.

- `DockManager.Theme` is a dependency property, so you can bind it. Setting the current value again still reapplies the theme.
- Put `Dock.*` keys, such as `DockTheme.AccentKey`, in `DockManager.Resources` to override theme brushes. The overrides win over the palette after the theme changes and also reach floating windows, the guide overlay, the auto-hide flyout, and toolbar windows.
- New `DockManager.SavedLayout` string property that binds both ways. Setting it loads that layout, also before the window is shown. Null or an empty string does nothing. Text that can't be read throws `DockLayoutException` and leaves the current layout alone.
- New `DockCommands.SaveLayout` writes the current layout into `SavedLayout` without loading it again.
- New `DockCommands.LoadLayout` reloads `SavedLayout`, even when the text hasn't changed. A non-empty `CommandParameter` is loaded instead and stored in `SavedLayout`. The command is disabled while `SavedLayout` is null or empty.
- Items from `DocumentsSource` and `ToolsSource` without an explicit template now pick up an implicit `DataTemplate` from a parent, such as `Window.Resources`.
- New documentation articles on styling, declaring a layout in XAML, and MVVM, with sample code for each.

## 0.9.0

First public version.

- Horizontal and vertical splits with weighted sizes.
- Document and tool tab groups. Drag a tab to reorder it, tear it off into a floating window, or drop it on a dock guide.
- Floating windows that can be maximized and docked back.
- Auto-hide strips and flyouts for tool panes.
- Hide a pane and show it again; hidden panes stay in the saved layout.
- Save and load the layout as JSON, including floats, auto-hide, hidden panes, and toolbars.
- Toolbars on the main window, floating windows, and panes, with a customize menu.
- Ctrl+Tab pane switcher and commands for close, float, and auto-hide.
- Dark, light, and blue themes.
- Events for layout changes, closing, pane state, activation, and content mount and unmount.
- Bind documents and tools to a collection with a data template per item.
