local Webhook = "https://discord.com/api/webhooks/821559791843409951/U5gKmSBK_m6Qvj_JQgF-bscLhlvL7oEiWptUtWXYqwc1_cIW0PgVSVQ-0PsiIoZVTrJ5" 

local function rgbToHex(rgb)
local hexadecimal = '0X'
for key, value in pairs(rgb) do
   local hex = ''
   while(value > 0)do
local index = math.fmod(value, 16) + 1
value = math.floor(value / 16)
hex = string.sub('0123456789ABCDEF', index, index) .. hex
end
if(string.len(hex) == 0)then
hex = '00'
elseif(string.len(hex) == 1)then
hex = '0' .. hex
end
hexadecimal = hexadecimal .. hex
end
return hexadecimal
end

local Headers = {["content-type"] = "application/json"}
local Chat = game:GetService("Players").LocalPlayer.PlayerGui.Chat.Frame.ChatChannelParentFrame["Frame_MessageLogDisplay"].Scroller

Chat.ChildAdded:Connect(function(instance)
   if string.find(instance.TextLabel.Text,"[Server]") then
       local TextColor3 = instance.TextLabel.TextColor3
       local RGB = {TextColor3.R*255,TextColor3.G*255,TextColor3.B*255}
       local Hex = tonumber(rgbToHex({RGB[1],RGB[2],RGB[3]}))
       local Info = {["content"] = "",["embeds"] = {{["title"] = instance.TextLabel.Text,["color"] = Hex,["fields"] = {}},}}
       local Info = game:GetService("HttpService"):JSONEncode(Info)
       local HttpRequest = http_request;
       if syn then HttpRequest = syn.request else HttpRequest = http_request end
           HttpRequest({Url=Webhook, Body=Info, Method="POST", Headers=Headers})
   end
end)
