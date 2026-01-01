local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "PAINEL ADM SNINE",
   LoadingTitle = "Carregando SNINE Hub...",
   LoadingSubtitle = "by Gemini",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "SnineAdmin",
      FileName = "Config"
   }
})

local PlayerTab = Window:CreateTab("Principal", 4483362458) -- Ícone de players
local Section = PlayerTab:CreateSection("Gerenciamento de Jogadores")

local SelectedPlayer = nil
local PlayerNames = {}

-- Função para atualizar a lista de jogadores
local function GetPlayerNames()
    local names = {}
    for _, v in pairs(game.Players:GetPlayers()) do
        if v ~= game.Players.LocalPlayer then
            table.insert(names, v.Name)
        end
    end
    return names
end

-- Dropdown que atualiza automaticamente
local PlayerDropdown = PlayerTab:CreateDropdown({
   Name = "Selecione um Jogador",
   Options = GetPlayerNames(),
   CurrentOption = {""},
   MultipleOptions = false,
   Flag = "PlayerDropdown",
   Callback = function(Option)
      SelectedPlayer = Option[1]
   end,
})

-- Loop para atualizar os nomes na lista a cada 5 segundos (Auto-update)
task.spawn(function()
    while true do
        task.wait(5)
        PlayerNames = GetPlayerNames()
        PlayerDropdown:Refresh(PlayerNames, true)
    end
end)

local AdminSection = PlayerTab:CreateSection("Comandos de Administrador")

-- Função Genérica para Comandos
local function ExecuteAdmin(cmd)
    if SelectedPlayer then
        -- Envia o comando via chat (padrão de Admin Panels de Steal a Brainrot)
        game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/" .. cmd .. " " .. SelectedPlayer, "All")
        Rayfield:Notify({
            Title = "Comando Enviado",
            Content = "Executado " .. cmd .. " em " .. SelectedPlayer,
            Duration = 3,
            Image = 4483362458,
        })
    else
        Rayfield:Notify({
            Title = "Erro",
            Content = "Selecione um jogador primeiro!",
            Duration = 3,
            Image = 4483362458,
        })
    end
end

-- Botões de Ação
PlayerTab:CreateButton({
   Name = "Kill (Matar)",
   Callback = function() ExecuteAdmin("kill") end,
})

PlayerTab:CreateButton({
   Name = "Freeze (Congelar)",
   Callback = function() ExecuteAdmin("freeze") end,
})

PlayerTab:CreateButton({
   Name = "Kick (Expulsar)",
   Callback = function() ExecuteAdmin("kick") end,
})

PlayerTab:CreateButton({
   Name = "Fling (Arremessar)",
   Callback = function() ExecuteAdmin("fling") end,
})

Rayfield:Notify({
   Title = "SNINE Inicializado",
   Content = "O menu está pronto para uso.",
   Duration = 5,
})
