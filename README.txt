Clipboard Viewer

This small application shows the contents of any clipboard.
A combo box lets you choose the clipboard to be examined (either from the built-in list, or by entering the name of an arbitrary clipboard);
the list of types on the selected clipboard are displayed in a table view; and clicking on a type in the table view shows the data for that type in a text view.

The app has six custom classes:

PasteboardController
The central controller object driving the UI and getting data from the clipboard. This class shows:
- Interacting with the clipboard (getting types list; getting data for a given type; checking to see if pasteboard was changed between accesses).
- Interacting with the simple UI of the app, including an NSComboBox.
- Use of simple properties.
- User interface validation.
- Presenting errors.
- Using NSSavePanel as a sheet.

LazyDataTextStorage
Subclass of NSTextStorage, used as the backing store for the NSTextView which displays the contents.
Upon loading the nib, the NSTextStorage for this view is replaced with an instance of LazyDataTextStorage.
LazyDataTextStorage uses a string and an attributes dictionary for its backing store:
the string is retained, not copied, allowing the three lazily-evaluated NSStringsubclasses (below) to actually behave lazily.
Note that LazyDataTextStorage is not editable, and thus it overrides the editing methods of NSTextStorage to do nothing.

ASCIIString, HexString, and HexAndASCIIString
Three NSString subclasses for the three ways clipboard data is displayed in the application.
These classes are ordered by complexity:
ASCIIString is a very simple example of an NSString subclass, as is HexString.
Meanwhile, HexAndASCIIString uses a more complicated implementation of its getCharacters:range: method to interpret 10-byte sequences as lines and only process the lines that are needed.

PasteboardTypeTransformer
A subclass of NSValueTransformer.
This class converts pasteboard types based on four-character codes (like the old HFS type and creator codes) to be a bit more human-readable.
It also interprets the four bytes as native characters and puts them at the beginning of the pasteboard type's string.
Other pasteboard types are just passed through.
Although the conversion looks sticky, this is a reasonably good example of creating one's own value transformer.

