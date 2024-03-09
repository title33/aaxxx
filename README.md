local HttpService = game:GetService("HttpService")
local Webhook_URL = "https://discord.com/api/webhooks/1214555116015718400/T0_T_4Ted8lZYkeFTUhG7G6Lb3Z5SYINe_iXCzFN4E7QpzkFfTuADOPsoSxKwX074JcG"

local lastCheck = tick()
local ExistingItems = {}

for _, v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
  ExistingItems[v.Name] = true
end

while wait() do
  local Backpack = game.Players.LocalPlayer.Backpack
  local NewItem = nil

  for i, v in pairs(Backpack:GetChildren()) do
    if v.Added > lastCheck and not ExistingItems[v.Name] then
      NewItem = v.Name
      break
    end
  end

  if NewItem then
    ExistingItems[NewItem] = true

    local req = requestfunc({
      Url = Webhook_URL,
      Method = 'POST',
      Headers = {
        ['Content-Type'] = 'application/json'
      },
      Body = HttpService:JSONEncode({
        ["content"] = "",
        ["embeds"] = {{
          ["title"] = "มีไอเท็มใหม่ใน Backpack ของคุณ!",
          ["description"] = "ชื่อไอเท็ม: " .. NewItem
        }}
      })
    })
  end

  lastCheck = tick()
end
