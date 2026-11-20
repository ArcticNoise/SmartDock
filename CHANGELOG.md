# Changelog

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
