# Exile-AutorunFixes

This fixes several bug and exploits related to the autorun.
- Disables Autorun in Territory. (VG exploit)
- Disables Autorun while playing russian roulette.
- Disables Autorun while in Combat. Remove the first If statement in ExileClient_system_autoRun_canAutoRun if you don't want to disable autorun in combat!
- Disables Autorun while doing certain animations to fix a animation skipping exploit.
- "Fixes" the "slide" bug while holstering a weapon and pressing autorun.
- "Fixes" the instant weapon draw while pressing 1 or 2 while autorunning and then pressing a movement button. 

# Installation

1. Create two customcode overrides for ExileClient_system_autoRun_canAutoRun.sqf and ExileClient_system_autoRun_stop.sqf with the code from github.

2. Add this to your init.sqf in your missionfile.

		WeaponHolsterAutoRunBlock = false;
	
3. Make a new override for ExileClient_gui_hud_event_onKeyUp.sqf or change the existing one. (Name can be different if you use Vectorbuilding etc.)

4. Search for this part.

		case 0x20:
		{
			if (ExileClientIsAutoRunning) then
			{
				call ExileClient_system_autoRun_stop;
				_stopPropagation = true; 
			};
		};

And change it to:
	
	case 0x20:
	{
		if (ExileClientIsAutoRunning) then
		{
			call ExileClient_system_autoRun_stop;
			[] spawn {WeaponHolsterAutoRunBlock = true; sleep 3; WeaponHolsterAutoRunBlock = false;};
			_stopPropagation = true; 
		};
	};

5. Search for this second part

		case 0x05: 	
		{ 
			if !(ExileClientIsHandcuffed || ExileIsPlayingRussianRoulette) then 
			{
				if (ExileClientIsInConstructionMode) then
				{
					if !(ExileClientConstructionKitClassName isEqualTo "Exile_Item_Flag") then 
					{
						ExileClientConstructionModePhysx = !ExileClientConstructionModePhysx;
						[] call ExileClient_gui_constructionMode_update;
					};
				}
				else
				{
					if (currentWeapon player != "") then
					{
						ExileClientPlayerHolsteredWeapon = currentWeapon player;
						player action["switchWeapon", player, player, 100];
					}
					else 
					{
						if (ExileClientPlayerHolsteredWeapon != "") then
						{
							player selectWeapon ExileClientPlayerHolsteredWeapon;
						};
					};
				};
			};
			_stopPropagation = true;
		};
And change it to:

	case 0x05: 	
	{ 
		if !(ExileClientIsHandcuffed || ExileIsPlayingRussianRoulette) then 
		{
			if (ExileClientIsInConstructionMode) then
			{
				if !(ExileClientConstructionKitClassName isEqualTo "Exile_Item_Flag") then 
				{
					ExileClientConstructionModePhysx = !ExileClientConstructionModePhysx;
					[] call ExileClient_gui_constructionMode_update;
				};
			}
			else
			{
				if (currentWeapon player != "") then
				{
					ExileClientPlayerHolsteredWeapon = currentWeapon player;
					player action["switchWeapon", player, player, 100];
					[] spawn {WeaponHolsterAutoRunBlock = true; sleep 3; WeaponHolsterAutoRunBlock = false;};
				}
				else 
				{
					if (ExileClientPlayerHolsteredWeapon != "") then
					{
						player selectWeapon ExileClientPlayerHolsteredWeapon;
						[] spawn {WeaponHolsterAutoRunBlock = true; sleep 3; WeaponHolsterAutoRunBlock = false;};
					};
				};
			};
		};
		_stopPropagation = true;
	};
