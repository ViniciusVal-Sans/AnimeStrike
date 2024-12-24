-- Script de Farm Automático para Anime Strike

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local attackCooldown = false

-- Função para encontrar o inimigo mais próximo
local function findClosestEnemy()
    local closestEnemy = nil
    local closestDistance = math.huge

    for _, enemy in pairs(workspace.Enemies:GetChildren()) do
        if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
            local distance = (humanoidRootPart.Position - enemy.HumanoidRootPart.Position).Magnitude
            if distance < closestDistance then
                closestDistance = distance
                closestEnemy = enemy
            end
        end
    end

    return closestEnemy
end

-- Função para atacar o inimigo
local function performAttack(enemy)
    if attackCooldown or not enemy then return end
    attackCooldown = true

    print("Atacando inimigo: " .. enemy.Name)

    -- Simular ataque básico
    local attackAnimation = Instance.new("Animation")
    attackAnimation.AnimationId = "rbxassetid://1234567890" -- Substituir pelo ID real
    local animator = character.Humanoid:FindFirstChild("Animator") or character.Humanoid:WaitForChild("Animator")
    local playAnim = animator:LoadAnimation(attackAnimation)
    playAnim:Play()

    -- Causar dano no inimigo
    enemy.Humanoid:TakeDamage(10)

    -- Cooldown de 1 segundo entre ataques
    wait(1)
    attackCooldown = false
end

-- Função de farm automático
local function autoFarm()
    while wait(0.1) do
        local enemy = findClosestEnemy()
        if enemy then
            -- Mover-se até o inimigo
            humanoidRootPart.CFrame = CFrame.new(enemy.HumanoidRootPart.Position + Vector3.new(0, 0, -3))
            wait(0.2) -- Esperar para se ajustar

            -- Atacar o inimigo
            performAttack(enemy)
        else
            print("Nenhum inimigo encontrado, aguardando...")
            wait(1) -- Esperar caso não haja inimigos
        end
    end
end

-- Iniciar o farm automático
autoFarm()
