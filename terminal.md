# Markup terminal

## starship

[`starship`](https://starship.rs/guide/#%F0%9F%92%AD-inspired-by) support many type of terminal with some awesome [preset](https://starship.rs/presets/)

## Make terminal auto-complete cabability

### `cmd`

Install [`clink`](https://chrisant996.github.io/clink/clink.html) first. Then install `starship`

### `powershell`

- Install `PSReadLine`

```powershell
Install-Module PSReadLine -Force
# open C:\Users\<UserName>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
notepad $PROFILE
# then add these lines and save
Invoke-Expression (&starship init powershell)
Import-Module PSReadLine

Set-PSReadLineOption -PredictionSource History

Set-PSReadLineOption -HistorySearchCursorMovesToEnd
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward
Set-PSReadLineKeyHandler -Chord "Ctrl+RightArrow" -Function ForwardWord

Set-PSReadLineOption -Colors @{ InlinePrediction = '#875f5f'}
```

### linux terminal

Install and use [fish](https://fishshell.com/) shell
