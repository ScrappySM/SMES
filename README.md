# SMES - A Naming Convention for Scrap Mechanic

**Scrap Mechanic Executor Scripting (SMES)** is a naming convention for Scrap Mechanic Lua scripting. It is designed to be simple, easy to use, and easy to implement in any Lua executor.

<br>

> [!NOTE]
> All types displayed here are not real! Vanilla Lua does not actually support type checking, so these are only here for illustration purposes.

## Todo

- [x] Filesystem
- [x] Console
- [x] Input
- [ ] HTTP
- [ ] Drawing
- [ ] Websocket
- [ ] Metatable
- [ ] Crypt

## Filesystem

### `isFile`

```lua
function isFile(path: string): boolean
```

Returns `true` or `false` depending on whether the _file_ exists at `path`.

#### Parameters

- `path` - The path to the file.

#### Example

```lua
writeFile("file.txt", "Hello, world!")
print(isFile("file.txt")) --> true
makeFolder("folder")
print(isFile("folder")) --> false
```

---

### `isFolder`

```lua
function isFolder(path: string): boolean
```

Returns `true` or `false` depending on whether the _folder_ exists at `path`.

#### Parameters

- `path` - The path to the folder.

#### Example

```lua
makeFolder("folder")
print(isFolder("folder")) --> true
writeFile("file.txt", "Hello, world!")
print(isFolder("file.txt")) --> false
```

---

### `writeFile`

```lua
function writeFile(path: string, data: string?): boolean
```

Creates a file and writes `data` (if not empty) to the file located at `path`. If the file already exists, it will overwrite its contents.

#### Parameters

- `path` - The path to the file.
- `data` - The data to write to the file.

#### Example

```lua
writeFile("file.txt", "Hello, world!")
print(readFile("file.txt")) --> Hello, world!
```

---

### `readFile`

```lua
function readFile(path: string): string
```

Returns the contents of the file located at `path`.

#### Parameters

- `path` - The path to the file.

#### Example

```lua
writeFile("file.txt", "Hello, world!")
print(readFile("file.txt")) --> Hello, world!
```

---

### `makeFolder`

```lua
function makeFolder(path: string): boolean
```

Creates a folder at `path` if it does not already exist. It will return `false` if the folder could not be created, but if it already exists, it will return `true`.

#### Parameters

- `path` - The path to the folder to create.

#### Example

```lua
makeFolder("folder")
writeFile("folder/file.txt", "Hello, world!")
print(isFile("folder/file.txt")) --> true
```

---

### `appendFile`

```lua
function appendFile(path: string, data: string): boolean
```

Appends `data` to the file located at `path`. This does not overwrite the previous contents.

#### Parameters

- `path` - The path to the file.
- `data` - The data to append to the file.

#### Example

```lua
writeFile("file.txt", "Hello, ")
appendFile("file.txt", "world!")
print(readFile("file.txt")) --> Hello, world!
```

---

### `listFiles`

```lua
function listFiles(path: string, showFolders: boolean?): {string}
```

Returns a list of all files in the folder located at `path`. The returned list contains full paths to the files.

#### Parameters

- `path` - The path to the folder.
- `showFolders` (optional) - If `true`, shows folders as well.

#### Example

```lua
makeFolder("folder")
writeFile("folder/file.txt", "Hello, world!")
writeFile("folder/file1.txt", "Hello, world!")
writeFile("folder/file2.txt", "Hello, world!")
writeFile("file3.txt", "Hello, world!")
writeFile("file4.txt", "Hello, world!")
print(table.foreach(listFiles("folder", false), print)) --> file3.txt file4.txt

local function descend(path, level)
    level = level or 0
    for _, file in ipairs(listFiles(path)) do
        print(string.rep("  ", level) .. file)
        if isFolder(file) then
            descend(file, level + 1)
        end
    end
end

descend(".")
```

---

### `delFile`

```lua
function delFile(path: string): boolean
```

Deletes the file located at `path`.

#### Parameters

- `path` - The path to the file.

#### Example

```lua
writeFile("file.txt", "Hello, world!")
print(isFile("file.txt")) --> true
delFile("file.txt")
print(isFile("file.txt")) --> false
```

---

### `delFolder`

```lua
function delFolder(path: string): boolean
```

Deletes the folder located at `path`.

#### Parameters

- `path` - The path to the folder.

#### Example

```lua
makeFolder("folder")
writeFile("folder/file.txt", "Hello, world!")
print(isFolder("folder")) --> true
delFolder("folder")
print(isFolder("folder")) --> false
```

---

### `loadFile`

```lua
function loadFile(path: string): {function?, string}
```

Loads a file at `path` as a function. If there are no errors, it returns the function; otherwise, it returns `nil` and an error message.

#### Parameters

- `path` - The path to the file to load.

#### Example

```lua
writeFile("file.lua", "return 1 + 1")
local func, err = loadFile("file.lua")
if not err then
    print(func()) --> 2
else
    print(err)
end
```

---

### `doFile`

```lua
function doFile(path: string)
```

Runs the contents of the file located at `path`.

#### Parameters

- `path` - The path to the file to execute.

#### Example

```lua
makeFile("test.txt", "_G.ranDoFile = true")
doFile("test.txt")
print(_G.ranDoFile) --> true
```

## Console

### `isConsoleOpened`

```lua
function isConsoleOpened(): boolean
```

Returns `true` or `false` depending on whether the console is opened.

#### Example

```lua
print(isConsoleOpened()) --> false
consoleCreate()
print(isConsoleOpened()) --> true
```

### `consoleCreate`

```lua
function consoleCreate()
```

Creates a console window attached to the game.

#### Example

```lua
print(isConsoleOpened()) --> false
consoleCreate()
print(isConsoleOpened()) --> true
```

---

### `consoleDestroy`

```lua
function consoleDestroy()
```

If the console is opened, it will destroy the console attached to the process.

#### Example

```lua
consoleCreate()
print(isConsoleOpened()) --> true
consoleDestroy()
print(isConsoleOpened()) --> false
```

---

### `consolePrint`

```lua
function consolePrint(data: string)
```

If the console is opened, this will output `data` into the console.

#### Example

```lua
consoleCreate()
consolePrint("Hello, world!")
--// Console
-- Hello, world!
```

---

### `consoleClear`

```lua
function consoleClear()
```

If the console is opened, this will clear all outputs from the console.

#### Example

```lua
consoleCreate()
consolePrint("Hello, world!")
consoleClear()
-- All contents of the console are now gone.
```

---

### `consoleSetTitle`

```lua
function consoleSetTitle(data: string)
```

If the console is opened, this will change the console's title to `data`.

#### Example

```lua
consoleCreate()
consoleSetTitle("Hello, world!")
-- The console title is now "Hello, world!"
```

---

### `consoleInput`

```lua
function consoleInput(): string
```

If the console is opened, this will wait for the user to input text into the console window and return the results.

#### Example

```lua
consoleCreate()
local input = consoleInput() --> Input "Hello, world!"
print(input) --> Hello, world!
```

## Input

> [!WARNING]
> These input functions must make sure to apply the input events only to the game window for security reasons. This prevents malicious scripts from locking the user into the game window by, for example, using `mouseMoveAbs` to move the mouse to the center, effectively trapping them in the game window. This is crucial for both security and a better user experience.

The **input** functions allow you to dispatch events to the game on behalf of the user.

### `isSMActive`

```lua
function isSMActive(): boolean
```

Returns `true` or `false` depending on whether the game is currently active.

#### Example

```lua
print(isSMActive())
```

---

### `mouseClick`

```lua
function mouseClick(ident: number, held: boolean)
```

Dispatches a mouse event to the game. `ident` indicates which mouse button to press, e.g., `1` for the left mouse button, `2` for the right mouse button, and `3` for the middle mouse button.

#### Parameters

- `ident` - The mouse button to press.

#### Example

```lua
mouseClick(1)
```

---

### `mouseHold`

```lua
function mouseHold(ident: number)
```

Dispatches a mouse hold event to the game. `ident` indicates which mouse button to press, e.g., `1` for the left mouse button, `2` for the right mouse button, and `3` for the middle mouse button. You will need to use `mouseRelease` to release the button after.

#### Parameters

- `ident` - The mouse button to press.

#### Example

```lua
mouseHold(1)
mouseRelease(1)
```

---

### `mouseRelease`

```lua
function mouseRelease(ident: number)
```

Dispatches a mouse release event to the game. `ident` indicates which mouse button to release, e.g., `1` for the left mouse button, `2` for the right mouse button, and `3` for the middle mouse button.

#### Parameters

- `ident` - The mouse button to release.

#### Example

```lua
mouseHold(1)
mouseRelease(1)
```

---

### `mouseMoveAbs`

```lua
function mouseMoveAbs(x: number, y: number)
```

Moves the mouse cursor to the specified position on the screen.

#### Parameters

- `x` - The x-position to move the mouse to.
- `y` - The y-position to move the mouse to.

#### Example

```lua
while not isSMActive() do
    wait(1)
end

mouseMoveAbs(100, 100)
```

---

### `mouseMoveRel`

```lua
function mouseMoveRel(x: number, y: number)
```

Moves the mouse cursor by the specified amount.

#### Parameters

- `x` - The x-amount to move the mouse by.
- `y` - The y-amount to move the mouse by.

#### Example

```lua
while not isSMActive() do
    wait(1)
end

mouseMoveRel(5, 5)
```

---

### `keyPress`

```lua
function keyPress(key: number, held: boolean)
```

Dispatches a key press event to the game. `key` indicates which key to press, e.g., `"A"` for the `A` key, `"B"` for the `B` key, etc.

---

## Miscellaneous

### `wait`

```lua
function wait(time: number)
```

Waits for `time` seconds before continuing the script.

#### Parameters

- `time` - The time to wait in seconds.

#### Example

```lua
print("Hello, world!")
wait(1)
print("Hello, world!") --> took 1 second to print.
```