# ReadIt: Easy File Reading in Zig

## What is ReadIt?
ReadIt is a small library that makes reading files in Zig simple. It has three functions:

1. `readFile` - Reads the whole file into memory.
2. `readFileSimple` - Reads up to 1024 bytes of a file (good for small files).
3. `readLines` - Reads a file line by line.

## How to Use

### 1. Read the Whole File

```zig
const std = @import("std");
const readit = @import("readit.zig");

pub fn main() !void {
    var gpa = std.heap.page_allocator;
    const content = try readit.readFile(gpa, "input.txt");
    defer gpa.free(content);

    std.debug.print("Full file:\n{s}\n", .{content});
}
```
- `readFile` loads the whole file into memory.
- You **must** free the memory with `gpa.free(content)` when done.

### 2. Read a Small File (Max 1024 Bytes)

```zig
const content = try readit.readFileSimple("input.txt");
std.debug.print("File contents:\n{s}\n", .{content});
```
- `readFileSimple` reads up to 1024 bytes.
- No need to free memory manually.

### 3. Read a File Line by Line

```zig
var lines = try readit.readLines(gpa, "input.txt");
defer {
    for (lines.items) |line| {
        gpa.free(line);
    }
    lines.deinit();
}

for (lines.items) |line| {
    std.debug.print("{s}\n", .{line});
}
```
- Reads the file one line at a time.
- Each line needs to be freed with `gpa.free(line)`.
- The list of lines must be deinitialized with `lines.deinit()`.

## Important Notes
- `readFile` and `readLines` allocate memory, so **you must free it**.
- `readFileSimple` does not need memory freeing.
- Make sure the file exists before reading it.

This library makes file reading as simple as Python’s `open().read()`. Have fun coding!

