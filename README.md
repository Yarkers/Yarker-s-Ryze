myHero = GetMyHero()

if myHero.charName ~= 'Ryze' then return end

function OnLoad()

Menu = scriptConfig("Yarkers Ryze","YarkersRyze")

Menu:addSubMenu("[ Ryze : Orbwalking ]","Orbwalking")
		OW:LoadToMenu(Menu.Orbwalking)

Menu:addSubMenu("[ Ryze : Combo ]","Combo")
		Menu.Combo:addParam("Mode","Combo mode",SCRIPT_PARAM_LIST,1,{"Mixed mode","Burst combo","Long combo"})
		Menu.Combo:addParam("R","Use R in combo",SCRIPT_PARAM_ONOFF,true)
		Menu.Combo:addParam("Item","Use item in combo",SCRIPT_PARAM_ONOFF,true)
		Menu.Combo:addParam("Ignite","Use ignite in combo",SCRIPT_PARAM_ONOFF,true)
		
		
function OnTick()
	ComboKey = Settings.combo.comboKey
	
if ComboKey then
		Combo(Target)
	end
	Checks()
	end
	
	function Combo(unit)
	if ValidTarget(unit) and unit ~= nil and unit.type == myHero.type then
		if Settings.combo.comboItems then
			UseItems(unit)
		end
		
		CastQ(unit)
		if Settings.combo.useR then CastR() end
		CastW(unit)
		CastE(unit)
	end
	
	if not Settings.combo.useAA then
		SOWi:DisableAttacks()
	elseif Settings.combo.useAA then
		SOWi:EnableAttacks()
	end
end

function CastQ(unit)
	if unit ~= nil and GetDistance(unit) <= SkillQ.range and SkillQ.ready then
		CastSpell(_Q, unit)
	end
end

function CastE(unit)
	if unit ~= nil and GetDistance(unit) <= SkillE.range and SkillE.ready then
		CastSpell(_E, unit)
	end
end

function CastW(unit)
	if unit ~= nil and SkillW.ready and GetDistance(unit) <= SkillW.range then
		CastSpell(_W, unit)
	end
end

function CastR()
	if SkillR.ready then
		if Settings.combo.useR == 1 then
			return
		elseif Settings.combo.useR == 2 and CountEnemyHeroInRange(SkillQ.range, myHero) >= 1 then
			CastSpell(_R) 
		elseif Settings.combo.useR == 3 and CountEnemyHeroInRange(SkillQ.range, myHero) >= 2 then
			CastSpell(_R)
		elseif Settings.combo.useR == 4 and CountEnemyHeroInRange(SkillQ.range, myHero) >= 3 then
			CastSpell(_R)
		elseif Settings.combo.useR == 5 and CountEnemyHeroInRange(SkillQ.range, myHero) >= 4 then
			CastSpell(_R)
		end
	end
end


function Checks()
	SkillQ.ready = (myHero:CanUseSpell(_Q) == READY)
	SkillW.ready = (myHero:CanUseSpell(_W) == READY)
	SkillE.ready = (myHero:CanUseSpell(_E) == READY)
	SkillR.ready = (myHero:CanUseSpell(_R) == READY)
