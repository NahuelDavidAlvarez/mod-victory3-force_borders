esta decision supuestamente hace que la IA de usa tenga como objetivo tener como amigos a los paises de sudamerica

usa_friends = {
    is_shown = {
        is_ai = no
    }
    
    possible = {
        always = yes
    }
    
    when_taken = {
    		c:USA = {
            
            set_secret_goal = { tcountry = c:ARG secret_goal = befriend } # Argentina
            set_secret_goal = { tcountry = c:CHL secret_goal = befriend } # Chile
            set_secret_goal = { tcountry = c:COL secret_goal = befriend } # Colombia/Nueva Granada
            set_secret_goal = { tcountry = c:PRU secret_goal = befriend } # Perú
            set_secret_goal = { tcountry = c:VEN secret_goal = befriend } # Venezuela
            set_secret_goal = { tcountry = c:URU secret_goal = befriend } # Uruguay
        }
    }    
 }

 ------------------------------

  eeuu_block = {
    is_shown = {
        is_ai = no
    }
    
    possible = {
        always = yes
    }
    when_taken = {
    	    c:USA = {
                activate_law = law_type:law_private_schools
                activate_law = law_type:law_private_health_insurance
                add_technology_researched = paddle_steamer
                create_power_bloc = {
                        name = "pb_great_america"
                        identity = identity_trade_league
                        map_color = { 120 216 230 }
                        principle = principle_external_trade_2
                        principle = principle_advanced_research_1
                        principle = principle_freedom_of_movement_2
                    }
                add_modifier = { // agregar modificadores, que debe estar seteados en la carpeta static_modifiers
                        name = USA_naval_superpower_ambition
                    }
                activate_law = law_type:law_no_migration_controls
                activate_law = law_type:law_private_schools
                }
    }    
 }

 ------------------------------

ejemplos de modificador que se aplica arriba

USA_naval_superpower_ambition = {
	icon = "gfx/interface/icons/timed_modifier_icons/modifier_statue_positive.dds"
    country_prestige_from_navy_power_projection_mult = 0.05
}

impulse_buff_inmigracion = {
	icon = "gfx/interface/icons/timed_modifier_icons/modifier_statue_positive.dds"
	
	country_legitimacy_base_add = 30
	interest_group_approval_add = 5
	country_authority_mult = 0.25
	country_weekly_innovation_add = 20
	country_minting_mult = 0.25
}


---------------------


# las traducciones Escribir con codificación UTF8 con BOM (necesario para Victoria 3)

Codificación UTF-8 con BOM: He usado un comando especial para forzar que el archivo lleve la "firma" (BOM) que el juego necesita para leer caracteres en español.
Formato exacto: He eliminado comentarios y espacios extra que a veces confunden al motor del juego.
Keys actualizadas: He incluido nombres para los Bloques de Poder (PB) y corregido las claves para que coincidan con tus archivos.txt


-----

# funcion anterior de reconocimiento chino, en la actualidad se usa el if para ver que nacion existe, para cuando china se fragmenta

reconocimiento_chino = {
	title = "reconocimiento_chino"
	description = "reconocimiento_chino_desc"
	
	is_shown = {
		is_ai = no
	}

	possible = {
		always = yes
	}

	when_taken = {
		c:CHI = {
			set_country_type = recognized
			set_variable = china_permanently_recognized
			custom_tooltip = "china_is_recognized_tt"

			activate_law = law_type:law_presidential_republic
			activate_law = law_type:law_autocracy
			activate_law = law_type:law_interventionism
            activate_law = law_type:law_protectionism
			activate_law = law_type:law_secret_police

			# Modificador "Impulso de Estabilidad"
			add_modifier = {
				name = impulse_estabilidad
				months = 36
			}

			# Lista de estados a transferir (Limpia enclaves y fragmentos)
			s:STATE_BEIJING = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_ZHILI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_SHANDONG = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_HENAN = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_SHANXI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_XIAN = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_JIANGSU = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_NANJING = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_SUZHOU = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_ZHEJIANG = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_NORTHERN_ANHUI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_SOUTHERN_ANHUI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_JIANGXI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_GUANGDONG = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_SHAOZHOU = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_GUANGXI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_FUJIAN = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_FORMOSA = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_HUNAN = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_GUIZHOU = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_YUNNAN = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_SICHUAN = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_CHONGQING = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_WESTERN_HUBEI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_EASTERN_HUBEI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_GANSU = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_NINGXIA = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_ALXA = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_QINGHAI = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_DZUNGARIA = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_TIANSHAN = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_SHENGJING = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_SOUTHERN_MANCHURIA = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_NORTHERN_MANCHURIA = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_HINGGAN = { every_scope_state = { set_state_owner = c:CHI } }
			s:STATE_OUTER_MANCHURIA = { every_scope_state = { set_state_owner = c:CHI } }
			#s:STATE_LHASA = { transfer_state_to = c:CHI }
			#s:STATE_NGARI = { transfer_state_to = c:CHI }
		}
	}
}
