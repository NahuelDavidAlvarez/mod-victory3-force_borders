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