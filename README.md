# mac-notes-export :spiral_notepad:
An AppleScript that can be used to export notes from "Notes" to a plain text file (.txt) with metadata (index, creation date, last modified, folder name). Only tested on macOS Big Sur 11.7.2. If you don't want to download mac_notes_export.scpt, you can copy below code into the Script Editor:
```
tell application "Notes"
	set allNotes to every note
	set totalNotesCount to (count of allNotes)
	set noteCounter to 1
	set filePathTXT to (path to desktop folder as text) & "mac_notes.txt"
	set fileRefTXT to open for access file filePathTXT with write permission
	set errorFilePathTXT to (path to desktop folder as text) & "mac_notes_export_errors.txt"
	set errorFileRefTXT to open for access file errorFilePathTXT with write permission
	repeat with currentNote in allNotes
		try
			set noteContent to body of currentNote
			set noteCreationDate to creation date of currentNote
			set noteModificationDate to modification date of currentNote
			set noteFolder to container of currentNote
			if noteFolder is not missing value then
				set folderName to name of noteFolder
			else
				set folderName to "NO FOLDER"
			end if
			if noteContent is "" then
				set noteContent to "NO CONTENT"
			end if
			write "NOTE " & noteCounter & "/" & totalNotesCount & linefeed to fileRefTXT
			write "CREATION DATE: " & noteCreationDate & linefeed to fileRefTXT
			write "MODIFICATION DATE: " & noteModificationDate & linefeed to fileRefTXT
			write "FOLDER: " & folderName & linefeed to fileRefTXT
			write "CONTENT:" & linefeed & noteContent & linefeed & linefeed to fileRefTXT
			set noteCounter to noteCounter + 1
		on error errorMessage
			write errorMessage & linefeed to errorFileRefTXT
		end try
	end repeat
	close access fileRefTXT
	close access errorFileRefTXT
end tell
```
