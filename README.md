# Exile-AutorunFixes

ExileClient_gui_hud_event_onKeyUp.sqf


case 0x20:
	{
		if (ExileClientIsAutoRunning) then
		{
			call ExileClient_system_autoRun_stop;
			_stopPropagation = true; 
		};
	};
