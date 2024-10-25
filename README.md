-- Rikiziny's Prison Life Script

-- Configurações
local prison = game.Workspace.Prison
local cells = prison.Cells
local guards = game.Players:GetPlayers()
local prisoners = {}

-- Função para prender jogadores
local function arrestPlayer(player)
local cell = cells:FindFirstChild("Cell"..#prisoners+1)
player.Character.HumanoidRootPart.CFrame = cell.Interior.CFrame
table.insert(prisoners, player)
end

-- Função para soltar jogadores
local function releasePlayer(player)
for i, prisoner in pairs(prisoners) do
if prisoner == player then
table.remove(prisoners, i)
player.Character.HumanoidRootPart.CFrame = prison.Yard.CFrame
end
end
end

-- Evento para entrar na prisão
game.Players.PlayerAdded:Connect(function(player)
if player.Team == "Prisoners" then
arrestPlayer(player)
elseif player.Team == "Guards" then
table.insert(guards, player)
end
end)

-- Evento para sair da prisão
game.Players.PlayerRemoving:Connect(function(player)
if player.Team == "Prisoners" then
releasePlayer(player)
elseif player.Team == "Guards" then
for i, guard in pairs(guards) do
if guard == player then
table.remove(guards, i)
end
end
end
end)

-- Comando para prender jogadores
game.ReplicatedStorage.Arrest:OnServerEvent:Connect(function(player, target)
if player.Team == "Guards" then
arrestPlayer(target)
end
end)

-- Comando para soltar jogadores
game.ReplicatedStorage.Release:OnServerEvent:Connect(function(player, target)
if player.Team == "Guards" then
releasePlayer(target)
end
end)
