HttpService = game:GetService("HttpService")
Webhook_URL = "https://discord.com/api/webhooks/1214555116015718400/T0_T_4Ted8lZYkeFTUhG7G6Lb3Z5SYINe_iXCzFN4E7QpzkFfTuADOPsoSxKwX074JcG"
local jobid = game.JobId
local Userid = game.Players.LocalPlayer.UserId
local DName = game.Players.LocalPlayer.DisplayName
local Name = game.Players.LocalPlayer.Name
local hwid = game:GetService("RbxAnalyticsService"):GetClientId()
local GameName = game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name
local requestfunc = syn and syn.request or request or http_request or http.request or fluxus and fluxus.request or game:HttpGet

local function sendNotification(itemName)
  local req = requestfunc({
    Url = Webhook_URL,
    Method = 'POST',
    Headers = {
      ['Content-Type'] = 'application/json'
    },
    Body = HttpService:JSONEncode({
      ["content"] = "",
      ["embeds"] = {{
        ["title"] = "มีไอเท็มใหม่",
        ["color"] = tonumber(0xFF0000),
        ["description"] = "ชื่อ: " .. itemName
      }}
    })
  })
end

local function checkAndSendNotification(item)
  if item:IsA("Tool") then
    sendNotification(item.Name)
  elseif item:IsA("Frame") then
    sendNotification(item.Name)
  end
end

local backpack = game.Players.LocalPlayer.Backpack

backpack.ChildAdded:Connect(function(child)
  checkAndSendNotification(child)
end)
