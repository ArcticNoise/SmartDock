# Changelog

## 0.16.9 (2026-11-28)

- `DockManager.FloatWindowBoundsChanged` fires once when a floating window finishes moving or resizing, and when SmartDock shows, maximizes, or restores one. A caption drag or an edge resize does not fire it on each mouse step. Docking the window does not fire it. `LayoutChanged` still does not fire for a float move.

## 0.16.8 (2026-11-27)

- Docking a toolbar back, or deleting it while it floats, now closes its window. Before, the window was only hidden and stayed alive, so every float and dock-back left one more window and its handles behind.
- Closing a floating pane, a floating toolbar, or an auto-hide flyout with a mouse click no longer leaves WPF reading the mouse from the destroyed window, which logged `Invalid window handle` errors. The window hides at once and is destroyed right after the click is handled.

## 0.16.7 (2026-11-27)

- Moving buttons onto a custom toolbar no longer fills the output with `System.Windows.Data Error: 26` when the host's toolbar style sets an `ItemTemplate` or `ItemTemplateSelector`. Resizing or docking the custom toolbar afterwards is clean too.
- A toolbar you just tore off no longer shows dock guides, or docks again, while the pointer is still close to where it tore off. Until the pointer is 48 DIP away, only a tray under the pointer can take it back, and releasing anywhere else leaves it floating. Dragging a toolbar that was already floating works as before.

## 0.16.6 (2026-11-27)

- Dragging a docked toolbar no longer shows the drop line, the dock guides, or a pane's edge bands until it floats. Sliding it along its band, or pulling it toward the document before it tears off, shows nothing. Once it floats, the guides work as before and stay if it docks again in the same drag. Where the toolbar slides and drops is unchanged.

## 0.16.5 (2026-11-27)

- A floating toolbar's orientation follows its grid. At least as many columns as rows is horizontal, including a square. Fewer columns than rows is vertical. Resizing no longer turns a wide grid vertical just because the window, caption included, is taller than it is wide.
- Undocking a pane by the right side or the middle of its caption keeps the cursor on the new float's caption.
- Moving a button onto a floating toolbar saves that window's size and line count, so closing the toolbar the button came from no longer resizes the other float.
- Dragging a toolbar no longer shows the pane dock bands or guides. A floating toolbar is drawn before its window is shown.
- A floating toolbar of two or more lines keeps the band fill between its buttons while resizing, after release, and after docking. Before, the gaps showed the theme's light brush or white.
- Resizing a float that holds several toolbars measures the whole window, so narrowing it to one column stacks the toolbars vertically again.
- Resizing a float of several toolbars with wide buttons into a vertical stack keeps the window at its content size. Before, the right side was clipped and an empty strip was left below the buttons.
- Resizing a float of toolbars with different button counts sizes each toolbar from its own buttons. A small edge drag on a six-button toolbar merged with a one-button toolbar no longer wraps it into a grid.
- A layout loaded before the dock is shown keeps its toolbars where it puts them. A toolbar's declared place is used only when the layout leaves that toolbar out. A floating toolbar of two or more lines keeps its line count and height.
- A toolbar dropped in the gap between the last start-aligned toolbar and an end-aligned one goes after the start-aligned toolbar and stays start-aligned. Before, it joined the end-aligned toolbars whenever it was longer than about the end-aligned toolbar. A drop over an end-aligned toolbar, or past it, still joins that group. `ToolBarDrop.OnEndRun`, `ToolBarBarRect.End`, and `ToolBarBands.JoinsEndRun` are new.
- Breaking change: the `ToolBarBands.FitGrid` overload that takes the wrap on screen no longer has `lineWraps`, which had no effect. Its last parameter is now `members`, the number of toolbars in the window, 1 by default. Drop the `lineWraps` argument from your calls.

## 0.16.4 (2026-11-26)

- A click on a floating toolbar's resize edge no longer replaces the toolbar template, so the host band fill stays and the window does not flash the default white background. The window background is painted in the theme chrome color.
- Right-clicking a toolbar button, or any other item, no longer opens the toolbar list. The list still opens on empty band, the grip, the chevron, and free tray space, docked or floating, including a vertical tray and an auto-hide flyout. An item's own menu is unchanged, and the overflow stays open.
- An empty toolbar is at least two icon slots along the band, floating and docked. A toolbar whose buttons are all hidden is empty too. The customize chevron sits at the far end of the row, including when a toolbar that does not show an overflow list is squeezed.
- Switching the dock theme updates the window background color of floating toolbars.
- Dragging a floating toolbar's edge by a few pixels no longer flips it between one row and one column. The wrap changes only when the new wrap is closer to the pointer than the current one by more than half a button. A horizontal drag does not snap to a vertical window that would end left of the pointer.
- Undocking a toolbar, dragging an icon, and showing the button grid no longer trace a binding error for `ToolBarTray.Orientation`. A host template that binds to the tray resolves while the toolbar is floating, and the grid's look probe is parented to a tray. The grid's overflow popup takes its placement target from the chevron, so that template does not bind by element name.
- A toolbar item key read from a single property no longer falls back to a binding when the item has no such property, including a dynamic object. A path that only some items have does not trace a data error for the others.
- A menu item sets its content alignment itself, so opening a context menu no longer traces a binding error looking for an ancestor items control.
- `ToolBarBands.FitGrid` has a public overload that takes the wrap already on screen (`holdVertical`, `holdColumns`, `holdRows`) and keeps it until another wrap is closer to the requested size by more than half a cell. The floating toolbar resize uses it.

## 0.16.3 (2026-11-26)

- A toolbar's gap in a band shrinks when the tray runs out of room. The toolbar starts overflowing into its chevron only after that gap is gone. The saved offset is unchanged, so making the window wider puts the toolbar back where it was. The chevron stays inside the tray. This applies to horizontal and vertical trays on the main window, on a pane, and on a pane in a floating window.
- Dropping a toolbar on an open auto-hide flyout docks it to that flyout when the pointer is over the flyout, including where a pane or a tray underneath used to take the drop. A pane the flyout does not cover still docks as before. The flyout's top edge is the first 24 DIP under its caption, as on a docked pane.
- Once a toolbar with `ShowOverflow` has overflowed, its buttons come back from the chevron as soon as the band is long enough again. Before, the chevron it had already shown kept some buttons in the overflow list.

## 0.16.2 (2026-11-26)

- Right-clicking a toolbar button that has its own context menu, or that opens its own popup on right-click, now opens only that menu or popup. The toolbar list no longer opens on top of it. The list still opens on an empty part of the band, the grip, the chevron, and a button with no menu of its own. This works on window trays, pane trays, and floating toolbars. Before, a plain button never opened the list, because WPF's `ToolBar` marks every press inside it as handled.
- Right-clicking an item in a toolbar's overflow keeps the overflow open, so the item's own menu opens, or the toolbar list if it has none. Before, the overflow closed and the list opened on the band underneath. To keep the overflow open, SmartDock marks an unhandled right press as handled at the overflow panel. A handler above the overflow panel only gets that press if it also listens for handled events.
- A floating toolbar resized into several lines keeps that line count when it docks to an edge with the same orientation: rows on a top or bottom tray, columns on a left or right one, pane trays included. Docking to the other orientation gives one line. The count is saved as `ToolBarEntry.Lines` and left out of the file when it is one line, so older layouts load unchanged. On a tray too short for the grid, the buttons that don't fit go to the chevron's overflow list. Reset goes back to one line.
- A floating toolbar resized into a grid keeps the background, border, corners, padding, and margin its template draws around the buttons. Before, a host-styled float lost its background as a grid and the caption seemed to run into the buttons. Theme and resource changes still reach the grid.
- The grip and the chevron line up with the button band, docked or floating, horizontal or vertical. A band inset by a template margin, a toolbar margin or height limit, or tray padding no longer leaves them 1 or 2 pixels taller than the band.
- On a vertical row the chevron's style width runs down the row, and the chevron spans the band like the grip does.
- A floating toolbar can dock to a pane shown in an open auto-hide flyout. Before, that drop failed with an error because the flyout lives in its own window.
- Toolbars no longer dock to the outside edges of a floating pane window. Dragging over its caption or sides shows no band, and dropping there leaves the toolbar floating. Panes inside that window still take toolbars. A saved layout with a toolbar on a floating window's edge loads that toolbar on the main window's top tray, and the next save drops `windowId`.
- The tray menu has a new Edit Custom Toolbars item after New Toolbar. It lists your custom toolbars so you can rename or delete them, and it is disabled when there are none. The dialog uses the dock theme.
- Add or Remove Buttons on a custom toolbar has Delete Toolbar next to Reset. It asks first, then removes the toolbar wherever it is. Deleting a floating toolbar closes its window. Built-in toolbars don't show the item.

## 0.16.1 (2026-11-26)

- A resized floating toolbar keeps each button at the size that button has in a single row or column. An image with no width or height stays that size. While the buttons are wrapped in a grid, a style that limits the toolbar's height or width is lifted, so the extra rows and columns stay whole. Those limits come back when the float is a single row or column again.
- Checking or unchecking Vertical measures the floating window again after the orientation change, including a toolbar the host restyles, so the new column or row fits at any display scale.
- The close button on a vertical float caption is 32/3 DIP (about 10.7), two thirds of the previous 16 DIP box, and its glyph is two thirds of the template's font size. A host template's own close button gets the same size. A horizontal caption keeps the template's width and font size.

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
