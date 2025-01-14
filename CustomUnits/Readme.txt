this mod allows you next things 
1. Add external animated parts to vehicles
2. Changing chassiss terrain interactions
3. Forbids certain meches/vehisles to spawn on certain biomes

main settings in mod.json
    "fixWaterHeight":true, - whether or not underwater terrain height should be fixed. If false hovers will not be able to move over water/deep water surface
    "maxWaterSteepness":30, - max underwater terrain steepness - if underwater terrain cell will have steepness grater than this value it will be fixed to 0 and cell will be marked as deep water
                              this needed for two things - hovers should not be affected by water cell steepness and not hovers should not be able to pass through this cell
    "waterFlatDepth":2, - if underwater cell have depth grater than this value x2 it will be lifted up.
    "deepWaterDepth":5  - if underwater cell have depth grater than this it will be marked as deep water. (Note: 5 it is greater than almost game's vehicle height)

DesignMask definition
new section 
	"Custom":{
		"CustomMoveCost":{
			"Hover": {    - name of move cost overriden section. You can set same name at chassis definition to override move cost for this chassis and biome
								additive for CAC temp masks. 
								For example radioactive_forest.Hover.moveCost = forest.Hover.moveCost + radioactive.Hover.moveCost - 1.0
				"moveCost":9999.9,
				"SprintMultiplier":9999.9
			}
		}
	}

VehicleChassis/Chassis
"CustomParts":{
    "AOEHeight": 55,  - this value will be added to y-coordinate of current position in AoE damage calculations (weapon/landmines/component's explosions). 
                        Can be altered runtime via CUAOEHeight actor's statistic value (float)
    "FiringArc":60, - if set and > 10 means vehicle firing arc in degrees and vehicle have to rotate toward target to fire.  
    "Unaffected":{  
      "DesignMasks":"true",   - if true chassis will be unaffected to all terrain design masks effects except move cost. Can be altered runtime via CUDesignMasksUnaffected actor's statistic value (boolean)
      "Pathing":"true",       - if true chassis will be unaffected by pathing limitations 
							(eg can climb vertcall surface, other actors collisions, not cause filmsy objects destruction on impact). 
							If choosed as melee target attacker melee (NOT AP) weapon always miss. Ignore terrain move cost.
              Can be altered runtime via CUPathingUnaffected actor's statistic value (boolean)
      "Fire":"true",         - if true chassis not receive damage passing burning terrain. Can be altered runtime via CUFireActorStatUnaffected actor's statistic value (boolean)
      "Landmines":"true",     - if true chassis not cause landmines to explode (still can receive AoE damage from landmines if explosion triggered by someone else).
                               Can be altered runtime via CULandminesUnaffected actor's statistic value (boolean)
      "MoveCostBiome":true   - if true unaffected by move modify per biome. Can be altered runtime via CUMoveCostBiomeUnaffected actor's statistic value (boolean)
    },
    "MoveCostModPerBiome":{  - dictionary of move cost modify per biome design mask
      "DesignMaskBiomeMartianVacuum":2.0,
      "DesignMaskBiomeLunarVacuum":10.0
    }, 
	"MoveCost":"Hover",     - move cost per design mask overridence (useless if Unaffected.Pathing is true).
                            Can be altered runtime via CUMoveCost actor's statistic value (string)
    "TieToGroundOnDeath":"true", - if true tied to ground on death (useful if prefab positions is altered by sections below)
	"HighestLOSPosition":{"x":0,"y":60,"z":0}, - offset for targeting
	"TurretAttach":{
		"offset":{"x":-0.6,"y":52.3,"z":5.5}, - offset for turret location, if omitted not touched
		"rotate":{"x":0,"y":0,"z":0} - rotation for turret location, if omitted not touched
	},
	"BodyAttach":{
		"offset":{"x":-0.6,"y":54,"z":0}  - offset for body location, if omitted not touched
	},
    "TurretLOS":{
		"offset":{"x":-0.6,"y":52.3,"z":5.5} - offset for turret line-of-sight location (usually same as TurretAttach), if omitted not touched
	},
	"LeftSideLOS":{
		"offset":{"x":-2,"y":54,"z":0} - offset for left side line-of-sight location, if omitted not touched
	},
	"RightSideLOS":{
		"offset":{"x":2,"y":54,"z":0} - offset for right side line-of-sight location, if omitted not touched
	},
	"leftVFXTransform":{
		"offset":{"x":-2,"y":54,"z":0} - offset for left vfx attach point (usually same as LeftSideLOS), if omitted not touched
	},
	"rightVFXTransform":{
		"offset":{"x":2,"y":54,"z":0} - offset for right vfx attach point (usually same as RightSideLOS), if omitted not touched
	},
	"rearVFXTransform":{
		"offset":{"x":0,"y":54,"z":-7} - offset for rear vfx attach point (usually same as RightSideLOS), if omitted not touched
	},
	"thisTransform":{
		"offset":{"x":0,"y":52,"z":0} - offset for main transform (as far as i can see not used for vehicles), if omitted not touched
	},
	"lightsTransforms":[ - offsets and rotations array for lights (i don't know why y-coordinate have to be doubled this values get from experiments)
		{"offset":{"x":-1.1,"y":103.8,"z":4.7},"rotate":{x:"60","y":0,"z":0}},
		{"offset":{"x":1.2,"y":103.8,"z":4.7},"rotate":{x:"60","y":0,"z":0}}
    ],
		"CustomParts":[  - array for external animated parts for chassis (all prefabs will be requested on loading dependences for chassis)
			{
				"prefab":"warrior_rotor_top_x10_brass", - prefab name
                            !READ NEXT NOTE CAREFULLY IF YOU EXPERIENCED PROBLEMS WITH SPAWNING YOUR PRESITANT OBJEJECTS USING CUSTOM UNITS MOD!
                            To make it work you must follow some rules 
                            1. You can't spawn prefab containing object with your models directly. You have to create empty object, name it as you want it to be referenced,
                            and only than add objects with your models as childs for this object. 
                            2. For name of parent object you have to use letters in LOWER case this is some sort of unity/game/modtek limitation.
                            3. You can search at the unity package i'm providing with this mod for examples how to create asset with prefabs engine can successfully load.
				"prefabTransform":{                     - prefab transformations
					"offset":  {"x":0,"y":1.4,"z":0.5},
					"scale":   {"x":1,"y":1,"z":1},
					"rotation":{"x":0, "y":0, "z":0}
				},
        "RequiredUpgrades":["Gear_Actuator_Coventry_Alpha"], - list of updrades required for this component to spawn. If list empty or omitted part will spawn always. 
                                                                If at least one component from list present, part will spawn.
                                                                For meches only MechChassisLocation checked. For vehicles all vehicle checked. 
        "VehicleChassisLocation":"Front",               - attach location for vehicles
        "MechChassisLocation":"RightArm",               - attach location for meches
                                                        NOTE! on location or entire unit destruction all spawned objects removing from game 
                                                        maybe later, i will implement proper physics.
        "MaterialInfo":{
          !READ NEXT NOTE CAREFULLY IF YOU EXPERIENCED PROBJEMS WITH TEXTURING YOUR LOADABLE OBJECTS!
          Some basics: model it is mesh + material. Material is shader + shader's settings relative to this model (textures is part of settings).
          Shader is set of instruction for GPU telling it how draw your model properly. 
          If you try to use standard unity shader for your prefabs - it will work wrong cause it does not know about game rendering features.
          So, to get your models to be textured properly you have next options:
          1. If you experienced enough you can write your own shader than will work correctly in game (but if you can, why are you reading this?)
          Fortunately i've found basic shader developers using to texture objects and its settings are very similar to standard one. 
          2. You can use UABE to replace standard unity's shader to game's one. (You can't import game's shader to your unity project cause absence of shader's source)
             But it needs additional post-production phase for your asset. After every change and reassemble you have to modify your asset to replace shader.
          3. You can include MaterialInfo to your custom part definition. If you do, my code while spawning objects will search through all models in your prefab for materials you point
             and replace their shaders to ones you points (for example i'm providing possibility to replace shader to game's standard one)
          "warrior_top_rotor_0Mat":{                              - name of material in your prefab to search
            "shader": "battletechstandartshader",                 - name of shader
            "shaderKeyWords":["_DETAIL_OVERLAY","_EMISSION","_METALLICGLOSSMAP","_NORMALMAP"] - list of shader keywords.
          }
        },
        
				"AnimationType":"SimpleRotator",        - animation type
				"AnimationData":{ - data specific for animation type
        "speed":5, - rotor blades rotetion speed
        "sound":"AudioEventList_motor_motor_buzzy_start", - sound for rotor 
        "axis":"y" - axis to rotate around (note it is object's axis not world's one it is depends on object's rotation)
        "rotateBone":"saw" - name of transform to rotate.
            Some basics: your prefab can consist of many child objects (models). 
            With SimpleRotator you can rotate whole set or just one of them. Look at king crab's chassis circle saw demo. 
        }  
			},
			{
				"prefab":"warrior_rotor_bottom_x10_brass",
				"prefabTransform":{
					"offset":  {"x":0,"y":1.2,"z":0.5},
					"scale":   {"x":1,"y":1,"z":1},
					"rotation":{"x":0, "y":0, "z":0}
				},
        "VehicleChassisLocation":"Front",
				"AnimationType":"SimpleRotator",
				"AnimationData":{"speed":-5,"axis":"y"}
			},
			{
				"prefab":"warrior_turret_x10_red",
				"prefabTransform":{
					"offset":  {"x":0,"y":0,"z":0},
					"scale":   {"x":20,"y":20,"z":20},
					"rotation":{"x":0, "y":0, "z":0}
				},
        "VehicleChassisLocation":"Turret",       - parent attach point (only have meaning only in visual plane)
        "VehicleChassisLocation":"Turret",       - parent attach point (only have meaning only in visual plane)
				"AnimationType":"Turret",                - turret rotates toward target on fire 
				"AnimationData":{
          "speed":10,                 - in idle state turret rotates +60 <-> - 60 degrees in horizontal plane
          "axis":"y"                  - reserved for future use
          "VehicleLocation": "Front", - all weapons in this location will be attached to with turret and their fire visuals will be played from its barrel
          "barrels":[                 - array of barrel transforms (only first barrel using now)
            {"offset":{"x":0,"y":-3,"z":3}}  - barrel offset/rotate/scale. Parent point is turret center
          ]
        }
        NOTE! You can attach to vehicle as many turrets as you want but they need have different VehicleLocation. Only first turret in each location will be used.
			},
			{
				"prefab":"",
				"prefabTransform":{
					"offset":  {"x":0,"y":0,"z":0},
					"scale":   {"x":1,"y":1,"z":1},
					"rotation":{"x":0, "y":0, "z":0}
				},
        "VehicleChassisLocation":"Left",
				"AnimationType":"WeaponMountPoint",  - weapon mount point animator, just stub. Having barrels as turret but not rotating to target. 
                                               Useful if you want to have different mount points for left and right side of vehicle 
                                               (vanilla have only two attach points turret and body, eg. weapon from left/right/front/rear locations in vanilla will be visually attached to body)
				"AnimationData":{
          "speed":1,                           - not used
          "VehicleLocation": "Left",          - same meaning as for Turret animator
          "barrels":[                         - same meaning as for Turret animator
            {"offset":{"x":0,"y":-3,"z":0}}
          ]
        }
			},
		]
}, 

you can control per-biome spawning via vehicle/mech tags
current supported values:
	NoBiome_generic,
	NoBiome_highlandsSpring,
	NoBiome_highlandsFall,
	NoBiome_lowlandsSpring,
	NoBiome_lowlandsFall,
	NoBiome_desertParched,
	NoBiome_badlandsParched,
	NoBiome_lowlandsCoastal,
	NoBiome_lunarVacuum,
	NoBiome_martianVacuum,
	NoBiome_polarFrozen,
	NoBiome_tundraFrozen,
	NoBiome_jungleTropical,
	NoBiome_urbanHighTech,

appendix A. Game's build-in audio events names in format '<name>':<id>
id - just for info purposes
 'AudioEventList_aircraft_aircraft_dropship_gencon_landing':3307102648
 'AudioEventList_aircraft_aircraft_leopard_destruction':556692128
 'AudioEventList_aircraft_aircraft_leopard_landing':1271550741
 'AudioEventList_aircraft_aircraft_leopard_takeoff':199263772
 'AudioEventList_aircraft_aircraft_leopard_touchAndGo':4220346206
 'AudioEventList_aircraft_aircraft_union_destruction':3671973990
 'AudioEventList_aircraft_aircraft_union_landing':3475508787
 'AudioEventList_vehicle_vehicle_explosion_a':3264067355
 'AudioEventList_vehicle_vehicle_explosion_b':3264067352
 'AudioEventList_vehicle_vehicle_gallant_engine_start':4190637255
 'AudioEventList_vehicle_vehicle_gallant_engine_stop':965956205
 'AudioEventList_vehicle_vehicle_gallant_start':1957306428
 'AudioEventList_vehicle_vehicle_gallant_stop':1500812688
 'AudioEventList_vehicle_vehicle_striker_engine_start':1747122798
 'AudioEventList_vehicle_vehicle_striker_engine_stop':661937998
 'AudioEventList_vehicle_vehicle_striker_start':2272730975
 'AudioEventList_vehicle_vehicle_striker_stop':2358986789
 'AudioEventList_vehicle_vehicle_tank_engine_start':335805546
 'AudioEventList_vehicle_vehicle_tank_engine_stop':61446978
 'AudioEventList_vehicle_vehicle_tank_start':1585469955
 'AudioEventList_vehicle_vehicle_tank_stop':423783297
 'AudioEventList_vehicle_vehicle_turret_interrupted':2305709755
 'AudioEventList_vehicle_vehicle_turret_start':1268444341
 'AudioEventList_vehicle_vehicle_turret_stop':843885591
 'AudioEventList_amb_amb_steam_engineeringRoom':2012392099
 'AudioEventList_ambiences_ambiences_start':1596568133
 'AudioEventList_ambiences_ambiences_stop':1987049447
 'AudioEventList_docking_docking_dock':1127131822
 'AudioEventList_docking_docking_release':111588762
 'AudioEventList_jump_jump_end':2752223339
 'AudioEventList_jump_jump_start':3244792144
 'AudioEventList_whoosh_whoosh_argo_large':2571020097
 'AudioEventList_whoosh_whoosh_argo_medium':1086353609
 'AudioEventList_whoosh_whoosh_argo_short':1302843434
 'AudioEventList_whoosh_whoosh_leopard_large':1370841601
 'AudioEventList_whoosh_whoosh_leopard_medium':1713558921
 'AudioEventList_whoosh_whoosh_leopard_small':3573500489
 'AudioEventList_whoosh_whoosh_long_hi':2556949102
 'AudioEventList_whoosh_whoosh_long_lo':2489838548
 'AudioEventList_whoosh_whoosh_long_mix':1301660715
 'AudioEventList_whoosh_whoosh_medium_hi':4184872571
 'AudioEventList_whoosh_whoosh_medium_lo':4117761961
 'AudioEventList_whoosh_whoosh_medium_mix':577441512
 'AudioEventList_whoosh_whoosh_short_hi':418135726
 'AudioEventList_whoosh_whoosh_short_lo':351025172
 'AudioEventList_whoosh_whoosh_short_mix':3722038635
 'AudioEventList_radio_radio_end':4172435892
 'AudioEventList_radio_radio_test01':3051689856
 'AudioEventList_radio_radio_test02':3051689859
 'AudioEventList_radio_radio_test03':3051689858
 'AudioEventList_vo_vo_play_argo_ambient':1697983434
 'AudioEventList_vo_vo_play_argo_group':2419063319
 'AudioEventList_vo_vo_play_argo_simgame':1791954385
 'AudioEventList_vo_vo_play_computer_ai':3491070856
 'AudioEventList_vo_vo_play_missions':3519092509
 'AudioEventList_vo_vo_play_pilot_player_character':2045319018
 'AudioEventList_vo_vo_play_pilots':3651348061
 'AudioEventList_vo_vo_play_procedural_mission':1810496138
 'AudioEventList_vo_vo_play_restoration_mission':662635667
 'AudioEventList_vo_vo_static_start_mission':650213631
 'AudioEventList_vo_vo_static_start_pilot':780605847
 'AudioEventList_vo_vo_static_stop_mission':498061635
 'AudioEventList_vo_vo_static_stop_pilot':236982011
 'AudioEventList_vo_vo_stop_argo_ambient':2960428520
 'AudioEventList_vo_vo_stop_argo_group':1872226985
 'AudioEventList_vo_vo_stop_argo_simgame':2543473511
 'AudioEventList_vo_vo_stop_missions':1531591975
 'AudioEventList_vo_vo_stop_pilots':1280583203
 'AudioEventList_vo_vo_stop_procedural_mission':4139172876
 'AudioEventList_vo_vo_stop_restoration_mission':2757205817
 'AudioEventList_environment_environment_beacon_loop_play':524534697
 'AudioEventList_environment_environment_beacon_loop_stop':2046951631
 'AudioEventList_environment_environment_beacon_pulse':4270851921
 'AudioEventList_environment_environment_dustdevils_loop_start':1564329446
 'AudioEventList_environment_environment_dustdevils_loop_stop':1910069030
 'AudioEventList_play_play_distant_battle_m010':2813920021
 'AudioEventList_play_play_fan':2757069037
 'AudioEventList_play_play_generator':3104499145
 'AudioEventList_play_play_laser_mining_machine':2853477536
 'AudioEventList_play_play_mission060_klaxon':3159316920
 'AudioEventList_play_play_orbital_gun':3135092258
 'AudioEventList_play_play_rubble_electrical':2848102585
 'AudioEventList_play_play_spores':1377559064
 'AudioEventList_play_play_urban_fan':1898457080
 'AudioEventList_play_play_urban_fountain_large':4127992541
 'AudioEventList_play_play_urban_fountain_medium':1401121053
 'AudioEventList_play_play_urban_fountain_small':3779719925
 'AudioEventList_play_play_urban_generator_coolant':2852242899
 'AudioEventList_play_play_urban_generator_electrical':2895017949
 'AudioEventList_play_play_urban_holograms':2004687783
 'AudioEventList_play_play_water_skirt_vent':3553859511
 'AudioEventList_play_play_waterfall_short_narrow':1601871927
 'AudioEventList_play_play_waterfall_short_wide':3075392599
 'AudioEventList_play_play_waterfall_tall_narrow':3485580056
 'AudioEventList_play_play_waterfall_tall_wide':2941377040
 'AudioEventList_play_play_artillery_impact':655314149
 'AudioEventList_play_play_artillery_projectile':1799047542
 'AudioEventList_play_play_dropPod_impact':3657599031
 'AudioEventList_play_play_dropPod_projectile':1023942412
 'AudioEventList_play_play_orbital_ppc':3188235375
 'AudioEventList_stop_stop_distant_battle_m010':319214499
 'AudioEventList_stop_stop_fan':503719263
 'AudioEventList_stop_stop_generator':3985384471
 'AudioEventList_stop_stop_laser_mining_machine':1898163914
 'AudioEventList_stop_stop_rubble_electrical':1587172455
 'AudioEventList_stop_stop_urban_fan':4021683170
 'AudioEventList_stop_stop_urban_fountain_large':1393668887
 'AudioEventList_stop_stop_urban_fountain_medium':2533969139
 'AudioEventList_stop_stop_urban_fountain_small':4141285539
 'AudioEventList_stop_stop_urban_generator_coolant':3265439909
 'AudioEventList_stop_stop_urban_generator_electrical':1245777447
 'AudioEventList_stop_stop_urban_holograms':2263294485
 'AudioEventList_stop_stop_water_skirt_vent':2942943201
 'AudioEventList_crab_crab_close_impact':1474530971
 'AudioEventList_crab_crab_close_move':2887032870
 'AudioEventList_crab_crab_opentoattack':3798826581
 'AudioEventList_crab_crab_opentoready':1934134290
 'AudioEventList_ducking_ducking_on_melee':1646489255
 'AudioEventList_eject_eject_launch':239187470
 'AudioEventList_eject_eject_projectile':3356178986
 'AudioEventList_foley_foley_long':1281966843
 'AudioEventList_foley_foley_medium':279517784
 'AudioEventList_foley_foley_short':2462372597
 'AudioEventList_hatchet_hatchet_latch':3971533133
 'AudioEventList_hatchet_hatchet_move':1016923602
 'AudioEventList_hatchet_hatchet_slide':4111452128
 'AudioEventList_hatchet_hatchet_sparks':728688223
 'AudioEventList_mech_mech_bodyfall':3638155978
 'AudioEventList_mech_mech_bodyfall_heavy':593568874
 'AudioEventList_mech_mech_bodyfall_light':2434904079
 'AudioEventList_mech_mech_bodyfall_medium':4235120740
 'AudioEventList_mech_mech_cockpit_explosion':1589129056
 'AudioEventList_mech_mech_coolant_vent':1007289689
 'AudioEventList_mech_mech_damage_burning_electrical_start':28447346
 'AudioEventList_mech_mech_damage_burning_electrical_stop':2604832682
 'AudioEventList_mech_mech_damage_burning_sparks_start':3786872438
 'AudioEventList_mech_mech_damage_burning_sparks_stop':350931958
 'AudioEventList_mech_mech_damage_bursts_sparks':4201573853
 'AudioEventList_mech_mech_destruction_a':3290458103
 'AudioEventList_mech_mech_destruction_b':3290458100
 'AudioEventList_mech_mech_destruction_c':3290458101
 'AudioEventList_mech_mech_engine_damaged_badly':1285733082
 'AudioEventList_mech_mech_engine_damaged_mildly':3177945601
 'AudioEventList_mech_mech_engine_powerdown':3882980581
 'AudioEventList_mech_mech_engine_powerdown_death':3583119990
 'AudioEventList_mech_mech_engine_powerup':2497018962
 'AudioEventList_mech_mech_engine_start':2535540750
 'AudioEventList_mech_mech_engine_stop':3401536366
 'AudioEventList_mech_mech_fire_large':792342065
 'AudioEventList_mech_mech_fire_medium':3793694553
 'AudioEventList_mech_mech_fire_small':855540089
 'AudioEventList_mech_mech_fire_small_internal':3769906135
 'AudioEventList_mech_mech_fire_stop':1514463188
 'AudioEventList_mech_mech_footstep':2435876175
 'AudioEventList_mech_mech_footstep_idle':4099323152
 'AudioEventList_mech_mech_impact01':1474674966
 'AudioEventList_mech_mech_impact02':1474674965
 'AudioEventList_mech_mech_impact03':1474674964
 'AudioEventList_mech_mech_impact04':1474674963
 'AudioEventList_mech_mech_jumpjets_heavy_start':3438577016
 'AudioEventList_mech_mech_jumpjets_heavy_stop':2763246980
 'AudioEventList_mech_mech_jumpjets_light_start':2671627645
 'AudioEventList_mech_mech_jumpjets_light_stop':2640150271
 'AudioEventList_mech_mech_jumpland':740494746
 'AudioEventList_mech_mech_metal_squeal':940202708
 'AudioEventList_mech_mech_part_break_a':2526891318
 'AudioEventList_mech_mech_part_break_b':2526891317
 'AudioEventList_mech_mech_part_break_c':2526891316
 'AudioEventList_mech_mech_surface_strike':1794745359
 'AudioEventList_mech_mech_water_enter':1431069237
 'AudioEventList_mech_mech_water_exit':1987229737
 'AudioEventList_motor_motor_buzzy_start':2339019304
 'AudioEventList_motor_motor_buzzy_stop':3768317428
 'AudioEventList_motor_motor_clicky_start':803403707
 'AudioEventList_motor_motor_clicky_stop':459470361
 'AudioEventList_motor_motor_sawserver_start':1069869742
 'AudioEventList_motor_motor_sawserver_stop':796140174
 'AudioEventList_move_move_mechanical':98366580
 'AudioEventList_move_move_mechanical_short':325303723
 'AudioEventList_on_on_ground_impact_hit':846702759
 'AudioEventList_on_on_ground_impact_miss':1134680808
 'AudioEventList_on_on_melee_impact':3039888256
 'AudioEventList_torso_torso_rotate_interrupted':2247726887
 'AudioEventList_torso_torso_rotate_start':1210558537
 'AudioEventList_torso_torso_rotate_stop':1356790499
 'AudioEventList_MU_MU_Play_GameplayMusic':441149344
 'AudioEventList_MU_MU_Play_MenuMusic':4039172695
 'AudioEventList_MU_MU_Stop_AllAudioExceptMusic':1917352548
 'AudioEventList_MU_MU_Stop_GameplayMusic':3160555914
 'AudioEventList_activeProbe_activeProbe_play':320379408
 'AudioEventList_activeProbe_activeProbe_priming_off':2099832552
 'AudioEventList_activeProbe_activeProbe_priming_on':2307477490
 'AudioEventList_activeProbe_activeProbe_stop':2963931826
 'AudioEventList_ui_ui_action_generic':1906840602
 'AudioEventList_ui_ui_action_gunnery':1191555991
 'AudioEventList_ui_ui_action_guts':991696128
 'AudioEventList_ui_ui_action_jump':1891400627
 'AudioEventList_ui_ui_action_notavailable':1202227231
 'AudioEventList_ui_ui_action_observation':747158411
 'AudioEventList_ui_ui_action_piloting':4065118337
 'AudioEventList_ui_ui_action_sprint':1398548603
 'AudioEventList_ui_ui_callout_inspired_buff':1400305465
 'AudioEventList_ui_ui_callout_toast':3658236246
 'AudioEventList_ui_ui_ecm_buff_down':1897676388
 'AudioEventList_ui_ui_ecm_buff_up':3808587679
 'AudioEventList_ui_ui_ecm_start':2551148966
 'AudioEventList_ui_ui_ecm_stop':4153254630
 'AudioEventList_ui_ui_esc_menu_hover':2866688670
 'AudioEventList_ui_ui_esc_menu_in':1920893261
 'AudioEventList_ui_ui_esc_menu_out':2402590420
 'AudioEventList_ui_ui_esc_menu_select':4034409072
 'AudioEventList_ui_ui_generic_confirm':390458608
 'AudioEventList_ui_ui_generic_hover':88994342
 'AudioEventList_ui_ui_mech_action_choose_yes':3970896206
 'AudioEventList_ui_ui_mech_action_hover':4247886413
 'AudioEventList_ui_ui_mech_choose_hover':1056171268
 'AudioEventList_ui_ui_mech_choose_off':3666879309
 'AudioEventList_ui_ui_mech_choose_on':4090541825
 'AudioEventList_ui_ui_mech_move':2538919975
 'AudioEventList_ui_ui_mech_move_path':2921029031
 'AudioEventList_ui_ui_mech_move_path_confirm':1156764138
 'AudioEventList_ui_ui_mech_move_rotate_start':3024088466
 'AudioEventList_ui_ui_mech_move_rotate_stop':3255711818
 'AudioEventList_ui_ui_mech_restart':2386294337
 'AudioEventList_ui_ui_mission_done':3192677011
 'AudioEventList_ui_ui_mission_fail':1911186247
 'AudioEventList_ui_ui_mission_popup_off':2686083129
 'AudioEventList_ui_ui_mission_popup_on':1450546573
 'AudioEventList_ui_ui_mission_withdraw':3881139189
 'AudioEventList_ui_ui_mp_chat_alert':879273697
 'AudioEventList_ui_ui_mp_go_to_mech_select':105996637
 'AudioEventList_ui_ui_mp_menu_hover':427470178
 'AudioEventList_ui_ui_mp_select_go':2015535309
 'AudioEventList_ui_ui_mp_select_mech_value_ok':2576795435
 'AudioEventList_ui_ui_mp_select_mech_value_over':3509850199
 'AudioEventList_ui_ui_objective_add':4069001851
 'AudioEventList_ui_ui_objective_done':1663464438
 'AudioEventList_ui_ui_objective_fail':2399597118
 'AudioEventList_ui_ui_overheat_alarm_1':3175092196
 'AudioEventList_ui_ui_overheat_alarm_2':3175092199
 'AudioEventList_ui_ui_overheat_alarm_3':3175092198
 'AudioEventList_ui_ui_overheat_imminent':2078777640
 'AudioEventList_ui_ui_overheat_shutdown_imminent':1337005591
 'AudioEventList_ui_ui_panels_down':2297788666
 'AudioEventList_ui_ui_panels_up':3780452249
 'AudioEventList_ui_ui_phase_1':3039138271
 'AudioEventList_ui_ui_phase_turn_me':958989884
 'AudioEventList_ui_ui_phase_turn_you':835085243
 'AudioEventList_ui_ui_radar_blip_detected':4157967717
 'AudioEventList_ui_ui_radar_blip_moving':2331000673
 'AudioEventList_ui_ui_salvage_received':4267392257
 'AudioEventList_ui_ui_settings_menu_check':1088701156
 'AudioEventList_ui_ui_settings_menu_uncheck':2948342627
 'AudioEventList_ui_ui_sim_back':1667154735
 'AudioEventList_ui_ui_sim_event_popup':2440455941
 'AudioEventList_ui_ui_sim_menu_appear':4029002589
 'AudioEventList_ui_ui_sim_menu_hover':116313428
 'AudioEventList_ui_ui_sim_menu_select':2274902470
 'AudioEventList_ui_ui_sim_object_hover':3103349592
 'AudioEventList_ui_ui_sim_object_select':4106159914
 'AudioEventList_ui_ui_sim_pilot_pic_appear':1878799779
 'AudioEventList_ui_ui_sim_platform_start':3223757106
 'AudioEventList_ui_ui_sim_platform_stop':1826771818
 'AudioEventList_ui_ui_sim_popup_newChassis':2997968017
 'AudioEventList_ui_ui_sim_rewardbox1':1996520511
 'AudioEventList_ui_ui_sim_rewardbox2':1996520508
 'AudioEventList_ui_ui_sim_submenu_appear':317544439
 'AudioEventList_ui_ui_sim_submenu_hover':3211003714
 'AudioEventList_ui_ui_sim_travel_loop_play':4185281064
 'AudioEventList_ui_ui_sim_travel_loop_stop':2267984026
 'AudioEventList_ui_ui_sim_travel_ping_play':2496623122
 'AudioEventList_ui_ui_sim_whoosh_room_change':961970741
 'AudioEventList_ui_ui_sim_whoosh_zoom':3648784138
 'AudioEventList_ui_ui_target_cycling':3846211843
 'AudioEventList_ui_ui_target_lock_bracket':2369329404
 'AudioEventList_ui_ui_target_lock_bracket_last':3689647655
 'AudioEventList_ui_ui_target_lock_hard':3520700967
 'AudioEventList_ui_ui_target_lock_soft':3537318368
 'AudioEventList_ui_ui_target_rotate_start':1279084164
 'AudioEventList_ui_ui_target_rotate_stop':1546761784
 'AudioEventList_ui_ui_target_sensor_lock_hard':796565048
 'AudioEventList_ui_ui_target_sensor_lock_soft':556931275
 'AudioEventList_ui_ui_target_sensor_lock_soft_off':3555642237
 'AudioEventList_ui_ui_target_sensor_sweep_start':884067730
 'AudioEventList_ui_ui_target_sensor_sweep_stop':1182849098
 'AudioEventList_ui_ui_weapon_choose_yes':378852106
 'AudioEventList_ui_ui_weapon_destroyed':4265165438
 'AudioEventList_ui_ui_weapon_disabled':3966917441
 'AudioEventList_ui_ui_weapon_hover':1761074433
 'AudioEventList_breakable_breakable_building_damage':1713355867
 'AudioEventList_breakable_breakable_concrete':2984704776
 'AudioEventList_breakable_breakable_concrete_barrier':3511346272
 'AudioEventList_breakable_breakable_concrete_large':495359612
 'AudioEventList_breakable_breakable_concrete_medium':2611577842
 'AudioEventList_breakable_breakable_concrete_small':2888588100
 'AudioEventList_breakable_breakable_dumpster':1423471327
 'AudioEventList_breakable_breakable_fence':3548602884
 'AudioEventList_breakable_breakable_fence_large':2368537064
 'AudioEventList_breakable_breakable_fence_medium':4169498886
 'AudioEventList_breakable_breakable_fence_small':749351936
 'AudioEventList_breakable_breakable_glass':387806267
 'AudioEventList_breakable_breakable_lightpole':2386213473
 'AudioEventList_breakable_breakable_metal':3490160970
 'AudioEventList_breakable_breakable_metal_large':1400066202
 'AudioEventList_breakable_breakable_metal_medium':2988863460
 'AudioEventList_breakable_breakable_metal_small':974947462
 'AudioEventList_breakable_breakable_radio_tower':1182306552
 'AudioEventList_breakable_breakable_vehicle':1201005715
 'AudioEventList_breakable_breakable_vehicle_fiery':2542997389
 'AudioEventList_collapse_collapse_large':3593681920
 'AudioEventList_collapse_collapse_large_fiery':1217697990
 'AudioEventList_collapse_collapse_large_fiery_tall':2836885728
 'AudioEventList_collapse_collapse_medium':510637070
 'AudioEventList_collapse_collapse_medium_fiery':1049034760
 'AudioEventList_collapse_collapse_short':740736451
 'AudioEventList_collapse_collapse_short_fiery':1962510813
 'AudioEventList_collapse_collapse_tiny':3377052157
 'AudioEventList_collapse_collapse_tiny_fiery':1350155819
 'AudioEventList_ecm_ecm_enter':855028813
 'AudioEventList_ecm_ecm_exit':1702294241
 'AudioEventList_explosion_explosion_coolant':1442258229
 'AudioEventList_explosion_explosion_electrical':2710565687
 'AudioEventList_explosion_explosion_large':2588735442
 'AudioEventList_explosion_explosion_medium':4165830412
 'AudioEventList_explosion_explosion_propane_tank':3916379071
 'AudioEventList_explosion_explosion_small':604083038
 'AudioEventList_fire_fire_large_180seconds':1928250444
 'AudioEventList_fire_fire_large_start':3618729104
 'AudioEventList_fire_fire_large_stop':1112110460
 'AudioEventList_fire_fire_medium_180seconds':160877140
 'AudioEventList_fire_fire_medium_start':211675768
 'AudioEventList_fire_fire_medium_stop':3138909316
 'AudioEventList_fire_fire_small_180seconds':1972692740
 'AudioEventList_fire_fire_small_start':788625640
 'AudioEventList_fire_fire_small_stop':2935850420
 'AudioEventList_impact_impact_weapon':1378694922
 'AudioEventList_small_small_building_collapse_large':1794037989
 'AudioEventList_small_small_building_collapse_med':174697090
 'AudioEventList_small_small_building_collapse_small':2154267453
 'AudioEventList_small_small_building_collapse_tiny':1798116218
 'AudioEventList_ac10_ac10_auto_single':1271390687
 'AudioEventList_ac10_ac10_auto_start':2794484601
 'AudioEventList_ac10_ac10_auto_stop':4142991059
 'AudioEventList_ac10_ac10_cannon':2644713222
 'AudioEventList_ac2_ac2_shot':1450394644
 'AudioEventList_ac20_ac20_auto_single':3029130770
 'AudioEventList_ac20_ac20_auto_start':1587432538
 'AudioEventList_ac20_ac20_auto_stop':1176261874
 'AudioEventList_ac20_ac20_cannon':580791825
 'AudioEventList_ac5_ac5_1stand2ndshot':2549964306
 'AudioEventList_ac5_ac5_3rdshot':1842726030
 'AudioEventList_flamer_flamer_start':328273805
 'AudioEventList_flamer_flamer_stop':2748237391
 'AudioEventList_gauss_gauss_chargeup':2917197164
 'AudioEventList_gauss_gauss_shoot':2452532288
 'AudioEventList_laser_laser_beam_stop_large':3209843651
 'AudioEventList_laser_laser_beam_stop_medium':2599849911
 'AudioEventList_laser_laser_beam_stop_small':3831089487
 'AudioEventList_laser_laser_large':4048453998
 'AudioEventList_laser_laser_medium':136592768
 'AudioEventList_laser_laser_small':3760557578
 'AudioEventList_lrm_lrm_launcher_end':607827825
 'AudioEventList_lrm_lrm_launcher_start':4237551394
 'AudioEventList_lrm_lrm_missile_launch':1647457025
 'AudioEventList_lrm_lrm_missile_launch_last':1762108332
 'AudioEventList_lrm_lrm_projectile_start':3368411099
 'AudioEventList_lrm_lrm_projectile_stop':2665274041
 'AudioEventList_machine_machine_gun_start':3916112878
 'AudioEventList_machine_machine_gun_stop':1663795150
 'AudioEventList_ppc_ppc_shoot':1071375452
 'AudioEventList_pulse_pulse_lrg_chargeup':383447416
 'AudioEventList_pulse_pulse_lrg_shoot':971720980
 'AudioEventList_pulse_pulse_med_chargeup':736367701
 'AudioEventList_pulse_pulse_med_shoot':3438267387
 'AudioEventList_pulse_pulse_sml_chargeup':4041000047
 'AudioEventList_pulse_pulse_sml_shoot':745217
 'AudioEventList_srm_srm_launcher_end':2830936084
 'AudioEventList_srm_srm_launcher_start':2788752519
 'AudioEventList_srm_srm_missile_launch':2876955656
 'AudioEventList_srm_srm_missile_launch_last':732096931
 'AudioEventList_srm_srm_projectile_start':766570538
 'AudioEventList_srm_srm_projectile_stop':2537716994