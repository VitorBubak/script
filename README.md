-- Script em ServerScriptService

local Players = game:GetService("Players")

-- Definição de missões de exemplo
local missoes = {
    {nivel = 1, objetivo = "Derrote 5 Slimes"},
    {nivel = 5, objetivo = "Derrote 10 Goblins"},
    {nivel = 10, objetivo = "Derrote 1 Boss Orc"}
}

-- Função para dar missão de acordo com o nível
local function darMissao(player)
    local nivel = player:FindFirstChild("Nivel")
    if not nivel then return end
    
    local missaoEscolhida
    for i = #missoes, 1, -1 do
        if nivel.Value >= missoes[i].nivel then
            missaoEscolhida = missoes[i]
            break
        end
    end

    if missaoEscolhida then
        print(player.Name .. " recebeu a missão: " .. missaoEscolhida.objetivo)
        -- Aqui você poderia armazenar no player, tipo:
        local missaoAtual = Instance.new("StringValue")
        missaoAtual.Name = "MissaoAtual"
        missaoAtual.Value = missaoEscolhida.objetivo
        missaoAtual.Parent = player
    end
end

-- Evento de clique (exemplo usando uma RemoteEvent)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local eventoMissao = Instance.new("RemoteEvent")
eventoMissao.Name = "PegarMissao"
eventoMissao.Parent = ReplicatedStorage

eventoMissao.OnServerEvent:Connect(function(player)
    darMissao(player)
end)
