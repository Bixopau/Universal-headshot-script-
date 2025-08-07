-- Criar GUI
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "MusashiModGUI"

-- Notificação temporária
local notif = Instance.new("TextLabel", ScreenGui)
notif.Size = UDim2.new(0, 250, 0, 40)
notif.Position = UDim2.new(0.5, -125, 0.1, 0)
notif.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
notif.TextColor3 = Color3.new(1, 1, 1)
notif.Font = Enum.Font.Gotham
notif.TextSize = 14
notif.BorderSizePixel = 0
notif.BackgroundTransparency = 0.3
notif.Text = "Criado por @mdsmusashi"
notif.AnchorPoint = Vector2.new(0, 0)
notif.TextStrokeTransparency = 0.7
notif.Visible = true

-- Some após 5 segundos
task.delay(5, function()
	notif:Destroy()
end)

-- Estética roxa: aplicar céu roxo
local sky = Instance.new("Sky", game.Lighting)
sky.SkyboxBk = "rbxassetid://98757147465135"
sky.SkyboxDn = "rbxassetid://98757147465135"
sky.SkyboxFt = "rbxassetid://98757147465135"
sky.SkyboxLf = "rbxassetid://98757147465135"
sky.SkyboxRt = "rbxassetid://98757147465135"
sky.SkyboxUp = "rbxassetid://98757147465135"

game.Lighting.Ambient = Color3.fromRGB(120, 0, 200)
game.Lighting.OutdoorAmbient = Color3.fromRGB(80, 0, 120)

-- Fundo com imagem
local backgroundImage = Instance.new("ImageLabel", ScreenGui)
backgroundImage.Size = UDim2.new(0, 240, 0, 200)
backgroundImage.Position = UDim2.new(0.5, -120, 0.5, -100)
backgroundImage.BackgroundTransparency = 1
backgroundImage.Image = "rbxassetid://81006743814733"
backgroundImage.Visible = false

-- Painel
local toggleFrame = Instance.new("Frame", backgroundImage)
toggleFrame.Size = UDim2.new(1, -20, 1, -20)
toggleFrame.Position = UDim2.new(0, 10, 0, 10)
toggleFrame.BackgroundColor3 = Color3.fromRGB(60, 0, 120)
toggleFrame.BorderSizePixel = 0
toggleFrame.AnchorPoint = Vector2.new(0, 0)
toggleFrame.BackgroundTransparency = 1
toggleFrame.Active = true
toggleFrame.Draggable = true

-- Título “musashi”
local labelMusashi = Instance.new("TextLabel", toggleFrame)
labelMusashi.Size = UDim2.new(1, -20, 0, 20)
labelMusashi.Position = UDim2.new(0, 10, 0, 5)
labelMusashi.BackgroundTransparency = 1
labelMusashi.Text = "musashi"
labelMusashi.TextColor3 = Color3.new(1, 1, 1)
labelMusashi.Font = Enum.Font.GothamBlack
labelMusashi.TextSize = 14
labelMusashi.TextXAlignment = Enum.TextXAlignment.Left

-- Botão CABEÇA
local headButton = Instance.new("TextButton", toggleFrame)
headButton.Size = UDim2.new(1, -20, 0, 40)
headButton.Position = UDim2.new(0, 10, 0, 30)
headButton.BackgroundColor3 = Color3.fromRGB(180, 80, 255)
headButton.Text = "CABEÇA: OFF"
headButton.TextColor3 = Color3.new(1, 1, 1)
headButton.Font = Enum.Font.GothamBlack
headButton.TextSize = 14
headButton.BorderSizePixel = 0
headButton.BackgroundTransparency = 0.1
headButton.AutoButtonColor = true

-- Botão TÓRAX
local torsoButton = Instance.new("TextButton", toggleFrame)
torsoButton.Size = UDim2.new(1, -20, 0, 40)
torsoButton.Position = UDim2.new(0, 10, 0, 80)
torsoButton.BackgroundColor3 = Color3.fromRGB(180, 80, 255)
torsoButton.Text = "TÓRAX: OFF"
torsoButton.TextColor3 = Color3.new(1, 1, 1)
torsoButton.Font = Enum.Font.GothamBlack
torsoButton.TextSize = 14
torsoButton.BorderSizePixel = 0
torsoButton.BackgroundTransparency = 0.1
torsoButton.AutoButtonColor = true

-- Botão flutuante redondo (sem texto)
local toggleGuiBtn = Instance.new("TextButton", ScreenGui)
toggleGuiBtn.Size = UDim2.new(0, 40, 0, 40)
toggleGuiBtn.Position = UDim2.new(0, 20, 0.5, -20)
toggleGuiBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 160)
toggleGuiBtn.Text = ""
toggleGuiBtn.BackgroundTransparency = 0.2
toggleGuiBtn.BorderSizePixel = 0
toggleGuiBtn.Name = "ToggleGUI"

local uicorner = Instance.new("UICorner", toggleGuiBtn)
uicorner.CornerRadius = UDim.new(1, 0)

-- Estado
local cabecaAtiva = false
local torsoAtivo = false

-- Modificar cabeça
local function modificarCabeca(head)
	if not head:FindFirstChild("OriginalSize") then
		local tag = Instance.new("Vector3Value", head)
		tag.Name = "OriginalSize"
		tag.Value = head.Size
	end
	head.Size = Vector3.new(10, 10, 10)
	head.Transparency = 1
	head.CanCollide = false
	head.Massless = true
	local face = head:FindFirstChildOfClass("Decal")
	if face then face:Destroy() end
end

-- Modificar torso
local function modificarTorso(torso)
	if not torso:FindFirstChild("OriginalSize") then
		local tag = Instance.new("Vector3Value", torso)
		tag.Name = "OriginalSize"
		tag.Value = torso.Size
	end
	torso.Size = Vector3.new(10, 10, 5)
	torso.Transparency = 1
	torso.CanCollide = false
	torso.Massless = true
end

-- Aplicar modificações
local function aplicarModificacoes()
	for _, plr in pairs(game.Players:GetPlayers()) do
		local char = plr.Character
		if char then
			local head = char:FindFirstChild("Head")
			local torso = char:FindFirstChild("UpperTorso") or char:FindFirstChild("Torso")
			if cabecaAtiva and head then modificarCabeca(head) end
			if torsoAtivo and torso then modificarTorso(torso) end
		end

		plr.CharacterAdded:Connect(function(char)
			wait(0.5)
			local head = char:FindFirstChild("Head")
			local torso = char:FindFirstChild("UpperTorso") or char:FindFirstChild("Torso")
			if cabecaAtiva and head then modificarCabeca(head) end
			if torsoAtivo and torso then modificarTorso(torso) end
		end)
	end
end

-- Atualizador contínuo
task.spawn(function()
	while true do
		if cabecaAtiva or torsoAtivo then
			aplicarModificacoes()
		end
		wait(3)
	end
end)

-- Clique cabeça
headButton.MouseButton1Click:Connect(function()
	if torsoAtivo then return end -- bloqueia se torso estiver ativo
	cabecaAtiva = not cabecaAtiva
	headButton.Text = "CABEÇA: " .. (cabecaAtiva and "ON" or "OFF")
	if cabecaAtiva then aplicarModificacoes() end
end)

-- Clique torso
torsoButton.MouseButton1Click:Connect(function()
	if cabecaAtiva then return end -- bloqueia se cabeça estiver ativa
	torsoAtivo = not torsoAtivo
	torsoButton.Text = "TÓRAX: " .. (torsoAtivo and "ON" or "OFF")
	if torsoAtivo then aplicarModificacoes() end
end)

-- Clique no botão redondo
toggleGuiBtn.MouseButton1Click:Connect(function()
	backgroundImage.Visible = not backgroundImage.Visible
end)
