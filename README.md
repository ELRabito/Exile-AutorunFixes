# Exile-AutorunFixes

# Installation

1. Create two customcode overrides for ExileClient_system_autoRun_canAutoRun.sqf and ExileClient_system_autoRun_stop.sqf.
2. Add this to your init.sqf in your missionfile.

	WeaponHolsterAutoRunBlock = false;
	
3. Make a new override for ExileClient_gui_hud_event_onKeyUp.sqf or change the existing one. (Name can be different if you use Vectorbuilding etc.)


Search for this part.

	case 0x20:
		{
			if (ExileClientIsAutoRunning) then
			{
				call ExileClient_system_autoRun_stop;
				_stopPropagation = true; 
			};
		};

Change to:
	
	case 0x20:
	{
		if (ExileClientIsAutoRunning) then
		{
			call ExileClient_system_autoRun_stop;
			[] spawn {WeaponHolsterAutoRunBlock = true; sleep 2.5; WeaponHolsterAutoRunBlock = false;};
			_stopPropagation = true; 
		};
	};
