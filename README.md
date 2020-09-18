# Exile-AutorunFixes



ExileClient_gui_hud_event_onKeyUp.sqf
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
