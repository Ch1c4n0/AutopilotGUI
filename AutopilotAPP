# Função para obter informações do Windows Autopilot com GroupTag
function Get-WindowsAutopilotInfoWithGroupTag($groupTag) {
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force
    Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Confirm:$false -Force:$true
    Install-Script get-windowsautopilotinfo -Confirm:$false -Force:$true
    get-windowsautopilotinfo -Online -GroupTag $groupTag
}

# Função para obter informações do Windows Autopilot e salvar em CSV
function Get-WindowsAutopilotInfoCSV {
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force
    Install-Script -Name Get-WindowsAutoPilotInfo -Force
    $serialNumber = (Get-WmiObject -Class Win32_BIOS).SerialNumber
    $folderPath = "C:\AUTOPILOT"
    if (-not (Test-Path -Path $folderPath)) {
        New-Item -ItemType Directory -Path $folderPath
    }
    $outputFile = "$folderPath\$serialNumber.csv"
    Get-WindowsAutoPilotInfo.ps1 -OutputFile $outputFile
    return $outputFile
}

# Criar a interface gráfica
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form
$form.Text = "Autopilot - Marcelo Goncalves v1.0 - Intune Lovers!"
$form.Size = New-Object System.Drawing.Size(400, 400)

# Adicionar mensagem
$labelTitle = New-Object System.Windows.Forms.Label
$labelTitle.Text = "Windows Autopilot"
$labelTitle.Font = New-Object System.Drawing.Font("Arial", 16)
$labelTitle.AutoSize = $true
$labelTitle.Location = New-Object System.Drawing.Point(100, 20)
$form.Controls.Add($labelTitle)

$buttonAutopilotGroupTag = New-Object System.Windows.Forms.Button
$buttonAutopilotGroupTag.Text = "Autopilot Online With Group Tag"
$buttonAutopilotGroupTag.Location = New-Object System.Drawing.Point(100, 120)
$buttonAutopilotGroupTag.Size = New-Object System.Drawing.Size(200, 30)
$form.Controls.Add($buttonAutopilotGroupTag)

$buttonAutopilotCSV = New-Object System.Windows.Forms.Button
$buttonAutopilotCSV.Text = "Windows Autopilot CSV"
$buttonAutopilotCSV.Location = New-Object System.Drawing.Point(100, 170)
$buttonAutopilotCSV.Size = New-Object System.Drawing.Size(200, 30)
$form.Controls.Add($buttonAutopilotCSV)

$labelStatus = New-Object System.Windows.Forms.Label
$labelStatus.Location = New-Object System.Drawing.Point(150, 50)
$labelStatus.Size = New-Object System.Drawing.Size(100, 20)
$form.Controls.Add($labelStatus)

# Evento de clique do botão Autopilot Online With Group Tag
$buttonAutopilotGroupTag.Add_Click({
    $inputForm = New-Object System.Windows.Forms.Form
    $inputForm.Text = "Enter Group Tag"
    $inputForm.Size = New-Object System.Drawing.Size(300, 150)

    $label = New-Object System.Windows.Forms.Label
    $label.Text = "Group Tag:"
    $label.Location = New-Object System.Drawing.Point(10, 20)
    $inputForm.Controls.Add($label)

    $textBox = New-Object System.Windows.Forms.TextBox
    $textBox.Location = New-Object System.Drawing.Point(50, 20)
    $textBox.Size = New-Object System.Drawing.Size(200, 20)
    $textBox.TextAlign = 'Center'
    $textBox.Anchor = [System.Windows.Forms.AnchorStyles]::Top -bor [System.Windows.Forms.AnchorStyles]::Left -bor [System.Windows.Forms.AnchorStyles]::Right
    $inputForm.Controls.Add($textBox)

    $buttonOK = New-Object System.Windows.Forms.Button
    $buttonOK.Text = "OK"
    $buttonOK.Location = New-Object System.Drawing.Point(100, 60)
    $buttonOK.Size = New-Object System.Drawing.Size(75, 30)
    $inputForm.Controls.Add($buttonOK)

    $buttonOK.Add_Click({
        $groupTag = $textBox.Text
        $inputForm.Close()
        $result = Get-WindowsAutopilotInfoWithGroupTag $groupTag
        [System.Windows.Forms.MessageBox]::Show("Autopilot information retrieved successfully.")
    })

    $inputForm.ShowDialog()
})

# Evento de clique do botão Windows Autopilot CSV
$buttonAutopilotCSV.Add_Click({
    $outputFile = Get-WindowsAutopilotInfoCSV
    [System.Windows.Forms.MessageBox]::Show("CSV file created at $outputFile")
})

# Exibir o formulário
[void]$form.ShowDialog()
