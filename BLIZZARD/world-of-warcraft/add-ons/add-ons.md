# World of Warcraft Addons

You can create a WoW Add-On very simply. Start creating your first Add-On by navigating [here].

Once you are done creating the Add-On script and name, you can download it, and put it into your interface/addons folder.


## Example _AddOns_ directory PATH.

> C:\Program Files (x86)\World of Warcraft\_retail_\Interface\AddOns

## Statically set buffs/debuffs to position in relation to PlayerFrame

```macro
function Movebuff() BuffFrame:ClearAllPoints()
BuffFrame:SetScale(1.1)
BuffFrame:SetPoint"CENTER",PlayerFrame,"CENTER",950,80) end
hooksecurefunc"UIParent_UpdateTopFramePositions",Movebuff)
Movebuff()
```

## UnitFrames colored by class

```addon-macro
local function colour(statusbar, unit)
        local _, class, c
        if UnitIsPlayer(unit) and UnitIsConnected(unit) and unit == statusbar.unit and UnitClass(unit) then
                _, class = UnitClass(unit)
                c = CUSTOM_CLASS_COLORS and CUSTOM_CLASS_COLORS[class] or RAID_CLASS_COLORS[class]
                statusbar:SetStatusBarColor(c.r, c.g, c.b)
             
        end
end
 
hooksecurefunc("UnitFrameHealthBar_Update", colour)
hooksecurefunc("HealthBar_OnValueChanged", function(self)
        colour(self, self.unit)
end)
 
local frame = CreateFrame("FRAME")
frame:RegisterEvent("GROUP_ROSTER_UPDATE")
frame:RegisterEvent("PLAYER_TARGET_CHANGED")
frame:RegisterEvent("PLAYER_FOCUS_CHANGED")
frame:RegisterEvent("UNIT_FACTION")
 
local function eventHandler(self, event, ...)
    if UnitIsPlayer("target") then
        c = RAID_CLASS_COLORS[select(2, UnitClass("target"))]
        TargetFrameNameBackground:SetVertexColor(c.r, c.g, c.b)
    end
    if UnitIsPlayer("focus") then
        c = RAID_CLASS_COLORS[select(2, UnitClass("focus"))]
        FocusFrameNameBackground:SetVertexColor(c.r, c.g, c.b)
    end
    if PlayerFrame:IsShown() and not PlayerFrame.bg then
        c = RAID_CLASS_COLORS[select(2, UnitClass("player"))]
        bg=PlayerFrame:CreateTexture()
        bg:SetPoint("TOPLEFT",PlayerFrameBackground)
        bg:SetPoint("BOTTOMRIGHT",PlayerFrameBackground,0,22)
        bg:SetTexture(TargetFrameNameBackground:GetTexture())
        bg:SetVertexColor(c.r,c.g,c.b)
        PlayerFrame.bg=true
    end
end
 
frame:SetScript("OnEvent", eventHandler)
 
for _, BarTextures in pairs({TargetFrameNameBackground, FocusFrameNameBackground}) do
    BarTextures:SetTexture("Interface\\TargetingFrame\\UI-StatusBar")
end
```