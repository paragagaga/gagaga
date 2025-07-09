--[[
    HUB SPEEDHACK - LOVE2D
    Ative/desative speedhack com multiplicador 200x pelo Hub interativo!
    Feito para fins educacionais. Use apenas em projetos próprios.
--]]

local speed_multiplier = 1.0
local speedhack_active = false

-- Para exemplo visual: objeto se movendo pela tela
local player = {
    x = 50,
    y = 300,
    speed = 120 -- pixels/segundo
}

-- HUB: estados do menu
local hub_open = true
local hub_message = "HUB ABERTO - [1] Ativar Speedhack | [2] Desativar | [H] Esconder Hub | [ESC] Sair"
local last_action = "Speedhack DESATIVADO"

function love.load()
    love.window.setTitle("Speedhack Hub")
    font = love.graphics.newFont(20)
    love.graphics.setFont(font)
end

-- Captura o update original
local original_update = love.update or function(dt) end

function love.update(dt)
    -- Aplica speedhack se ativo
    local time = dt
    if speedhack_active then
        time = dt * speed_multiplier
    end

    -- Para exemplo: mover "player"
    if not hub_open then
        if love.keyboard.isDown("right") then
            player.x = player.x + player.speed * time
            if player.x > love.graphics.getWidth() - 50 then player.x = love.graphics.getWidth() - 50 end
        end
        if love.keyboard.isDown("left") then
            player.x = player.x - player.speed * time
            if player.x < 0 then player.x = 0 end
        end
    end

    -- Chama update original
    original_update(time)
end

function love.keypressed(key)
    if hub_open then
        if key == "1" then
            speed_multiplier = 200.0
            speedhack_active = true
            last_action = "Speedhack ATIVADO (200x)"
        elseif key == "2" then
            speed_multiplier = 1.0
            speedhack_active = false
            last_action = "Speedhack DESATIVADO"
        elseif key == "h" then
            hub_open = false
        elseif key == "escape" then
            love.event.quit()
        end
    else
        if key == "h" then
            hub_open = true
        elseif key == "escape" then
            love.event.quit()
        end
    end
end

function love.draw()
    -- HUB
    if hub_open then
        love.graphics.setColor(0.08,0.08,0.12, 0.96)
        love.graphics.rectangle("fill", 70, 80, love.graphics.getWidth()-140, 270, 22)
        love.graphics.setColor(1,1,1)
        love.graphics.printf("=== SPEEDHACK HUB ===", 0, 100, love.graphics.getWidth(), "center")
        love.graphics.printf(hub_message, 0, 160, love.graphics.getWidth(), "center")
        love.graphics.setColor(0.5,1,0.5)
        love.graphics.printf("Status: "..last_action, 0, 220, love.graphics.getWidth(), "center")
        love.graphics.setColor(1,1,1)
        love.graphics.printf("Dica: Pressione [H] para esconder o Hub e controlar o player.\nUse as setas ← → para mover.", 0, 300, love.graphics.getWidth(), "center")
    end

    -- Player
    love.graphics.setColor(1,0.7,0.3)
    love.graphics.rectangle("fill", player.x, player.y, 50, 50)
    love.graphics.setColor(1,1,1)
    if not hub_open then
        love.graphics.printf("Pressione [H] para abrir o Hub novamente.", 0, 30, love.graphics.getWidth(), "center")
    end
end
