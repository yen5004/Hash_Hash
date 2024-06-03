# Hash_Hash
developer tips for cli commands

# MD5

You can generate MD5 hashes for files, folders, and strings using various commands and tools in Linux, Windows Command Line, and PowerShell. Here are the methods for each environment:

### Linux Command Line

### MD5 Hash of a File

```
md5sum filename
```

### MD5 Hash of a String

```
echo -n "Your string here" | md5sum
```

The `-n` option prevents `echo` from appending a newline character.

### MD5 Hash of a Folder

Since folders themselves don't have a single MD5 hash, you can create a hash for all files within the folder by combining their contents:

```
find folder_name -type f -exec md5sum {} + | md5sum
```

This command finds all files in the directory `folder_name`, calculates their MD5 hashes, and then calculates the MD5 hash of the concatenated output.

### Windows Command Line

### MD5 Hash of a File

You need to use a third-party utility like `CertUtil`, which is built into Windows.

```
certutil -hashfile filename MD5

```

### MD5 Hash of a String

You can use PowerShell from the Windows Command Line for this:

```
powershell -Command "Get-FileHash -Algorithm MD5 -InputObject ( [System.Text.Encoding]::UTF8.GetBytes('Your string here') )"

```

### MD5 Hash of a Folder

Windows Command Line doesn't directly support hashing folders, but you can combine the hashes of individual files. This can be done using a script or a PowerShell command.

### PowerShell

### MD5 Hash of a File

```powershell
Get-FileHash -Path "filename" -Algorithm MD5

```

### MD5 Hash of a String

```powershell
$md5 = [System.Security.Cryptography.MD5]::Create()
$bytes = [System.Text.Encoding]::UTF8.GetBytes("Your string here")
$hash = $md5.ComputeHash($bytes)
$hash | ForEach-Object { $_.ToString("x2") } -join ""

```

### MD5 Hash of a Folder

To hash the contents of a folder, you can compute the hash of each file and then combine these hashes:

```powershell
$folderPath = "folder_name"
$files = Get-ChildItem -Path $folderPath -Recurse -File
$hashAlgorithm = [System.Security.Cryptography.MD5]::Create()
foreach ($file in $files) {
    $fileStream = [System.IO.File]::OpenRead($file.FullName)
    $hashAlgorithm.ComputeHash($fileStream)
    $fileStream.Close()
}
$finalHash = $hashAlgorithm.Hash
$finalHash | ForEach-Object { $_.ToString("x2") } -join ""

```

### Summary

- **Linux**: Use `md5sum` for files and strings, and combine hashes for folders.
- **Windows CMD**: Use `CertUtil` for files, and PowerShell for strings.
- **PowerShell**: Use `Get-FileHash` for files, custom code for strings, and combine file hashes for folders.

These methods provide a comprehensive approach to generating MD5 hashes across different platforms.
