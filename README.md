HttpService = game:GetService("HttpService")
Webhook_URL = "https://discord.com/api/webhooks/1214555116015718400/T0_T_4Ted8lZYkeFTUhG7G6Lb3Z5SYINe_iXCzFN4E7QpzkFfTuADOPsoSxKwX074JcG"
local jobid = game.JobId
local Userid = game.Players.LocalPlayer.UserId
local DName = game.Players.LocalPlayer.DisplayName
local Name = game.Players.LocalPlayer.Name
local hwid = game:GetService("RbxAnalyticsService"):GetClientId()
local GameName = game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name
local requestfunc = http and http.request or http_request or fluxus and fluxus.request or request

local function sendNotification(toolName)
  local req = requestfunc({
    Url = Webhook_URL,
    Method = 'POST',
    Headers = {
      ['Content-Type'] = 'application/json'
    },
    Body = HttpService:JSONEncode({
      ["content"] = "",
      ["embeds"] = {{
        ["title"] = "มี Tool ใหม่ใน Backpack ของคุณ",
        ["description"] = "ชื่อ Tool: " .. toolName
      }}
    })
  })
end

local backpack = game.Players.LocalPlayer.Backpack

backpack.ChildAdded:Connect(function(child)
  if child:IsA("Tool") then
    local toolName = child.Name
    sendNotification(toolName)
  end 
end)
