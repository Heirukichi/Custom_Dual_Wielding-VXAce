#==============================================================================
# HEIRUKICHI SEALED DUAL WIELDING
#==============================================================================
# Version 1.0.1
# - Last updated: 06-26-2019 [MM-DD-YYYY]
# - Author: Heirukichi
#==============================================================================
# TERMS OF USE
#------------------------------------------------------------------------------
# This script is under the GNU General Public License v3.0. This means that:
# - You are free to use this script in both commercial and non-commercial games
#   as long as you give proper credits to me (Heirukichi) and provide a link to
#   my website;
# - You are free to modify this script as long as you do not pretend you wrote
#   this and you distribute it under the same license as the original.
#
# You can review the full license here: https://www.gnu.org/licenses/gpl-3.0.html
#
# In addition I'd like to keep track of games where my scripts are used so,
# even if this is not mandatory, I'd like you to inform me and send me a link
# when a game including my script is published.
# As I said, this is not mandatory but it really helps me and it is much
# appreciated.
#
# IMPORTANT NOTICE:
# If you want to distribute this code, feel free to do it, but provide a link
# to my website instead of pasting my script somewhere else.
#==============================================================================
# DESCRIPTION
#------------------------------------------------------------------------------
# This script allows you to prevent actors from equpping certain weapon types
# in the dual wielding slot, even if the could normally wield them.
#==============================================================================
# INSTRUCTIONS
#------------------------------------------------------------------------------
# Paste this script in your project below Materials. It should work fine even
# with other scripts that overwrite methods that are aliased in this one as
# long as you place this BELOW them.
# However, keep in mind that this script overwrites the include? method of
# Window_EquipItem class. It is highly unlikely to work with any other script
# that overwrites that method and it should be placed ABOVE any other script
# aliasing that method in order to reduce compatibility issues.
# Detailed instructions on how to configure this script are written in the
# Config module below.
#==============================================================================
# METHODS
#------------------------------------------------------------------------------
# This script interacts with the following classes and methods. Methods are
# listed using this rule:
#
#     * = aliased method
#    + = new method
#    ! = overwritten method
#------------------------------------------------------------------------------
# Window_EquipItem
#    ! include?
#------------------------------------------------------------------------------
# Game_Actor
#	 ! release_unequippable_items
#------------------------------------------------------------------------------
# Game_BattlerBase
#    * equippable?
#    * equip_wtype_ok?
#    + dual_wtype_allowed?
#	 + main_wtype_allowed?
#==============================================================================

$imported = {} if $imported.nil?
$imported["Heirukichi_CustomDW"] = true

module HCDW
 
    module Config
 
        #======================================================================
        # The following value contains your actors ID and their respective
        # sealed weapon for the dual wielding slot.
        # If no weapon type is listed it means each equippable weapon for that
        # actor can be equipped in the dual wielding slot as well.
        # The first number represents your actor ID while the number/s on the
        # right represent the sealed weapons.
        # It is possible to add or remove actors/weapons from the list.
        # To add more actors simply add a new line below the existing ones,
        # add your actor ID followed by a "=>" (without quotation marks) and
        # then list sealed weapon types ID for that actor between square
        # brackets separated by a comma.
        # Here is an example of how it should look like with more actors:
        #----------------------------------------------------------------------
        # Example:
        #----------------------------------------------------------------------
        # LOCKED_DUAL_WIELD_WEAPON_TYPES = {
        # 1 => [7],
        # 3 => [5, 2, 4],
        # 4 => [9, 1],
        # }
        #----------------------------------------------------------------------
        # In this example actor 1 cannot equip any weapon of category 7 in the
        # dual wielding slot. As you can see it is just a single weapon type
        # for that actor but you have to still use square brackets.
        # Actor 3 has more options locked. In fact weapon types 5, 2 and 4 are
        # locked and can only be equipped in the main weapon slow.
        # Similarly, actor 4 has two weapon types locked for the dual wielding
        # slot.
        #----------------------------------------------------------------------
        # Locked weapon types ID can be listed in any order inside the square
        # brackets.
        #----------------------------------------------------------------------
        # Similarly you can lock weapon types for the main hand slot changing
        # LOCKED_MAIN_HAND_WEAPON_TYPES. It follows the same rules as dual
        # wield slots.
        #----------------------------------------------------------------------
        # NOTE: Remember to put a comma at the end of each line.
        #======================================================================
     
        LOCKED_DUAL_WIELD_WEAPON_TYPES = {
            1 => [7],
        } # Do not remove this
        
        LOCKED_MAIN_HAND_WEAPON_TYPES = {
            1 => [1],
        } # Do not remove this
     
    end
 
    #==========================================================================
    # !! ---- WARNING! Do not modify after this point! -------------------- !!
    #==========================================================================
 
    def self.lckwd_wtype(type, id)
		return false unless Config::LOCKED_DUAL_WIELD_WEAPON_TYPES.keys.include?(id)
        return Config::LOCKED_DUAL_WIELD_WEAPON_TYPES[id].include?(type)
    end
	
	def self.lckmh_wtype(type, id)
		return false unless Config::LOCKED_MAIN_HAND_WEAPON_TYPES.keys.include?(id)
		return Config::LOCKED_MAIN_HAND_WEAPON_TYPES[id].include?(type)
	end
 
end

#==============================================================================
# * Window_EquipItem
#==============================================================================

class Window_EquipItem

    #==========================================================================
    # ! OVERWRITTEN METHOD
    #==========================================================================
 
    def include?(item)
        return true if item == nil
        return false unless item.is_a?(RPG::EquipItem)
        return false if @slot_id < 0
        return false if item.etype_id != @actor.equip_slots[@slot_id]
        return @actor.equippable?(item, @slot_id)
    end
 
end

#==============================================================================
# * Game_BattlerBase
#==============================================================================

class Game_BattlerBase

    #==========================================================================
    # * Aliased method
    #==========================================================================
 
    alias hrk_equippable_old  equippable?
    def equippable?(item, slot = 0)
        if ((slot > 1) || (!actor?))
            hrk_equippable_old(item)
        else
            return false unless item.is_a?(RPG::EquipItem)
            return false if equip_type_sealed?(item.etype_id)
            return equip_wtype_ok?(item.wtype_id, slot) if item.is_a?(RPG::Weapon)
            return equip_atype_ok?(item.atype_id) if item.is_a?(RPG::Armor)
            return false
        end
    end
 
    #==========================================================================
    # * Aliased method
    #==========================================================================
 
    alias hrk_wtype_ok_old equip_wtype_ok?
    def equip_wtype_ok?(wtype_id, slot = 0)
        flag = hrk_wtype_ok_old(wtype_id)
		if (actor? && flag)
			flag = dual_wtype_allowed?(wtype_id) if (slot == 1)
			flag = main_wtype_allowed?(wtype_id) if (slot == 0)
		end
        return flag
    end
 
    #==========================================================================
    # + New method
    #==========================================================================
 
    def dual_wtype_allowed?(wtype)
        return !HCDW.lckwd_wtype(wtype, @actor_id)
    end
 
    #==========================================================================
    # + New method
    #==========================================================================
	
	def main_wtype_allowed?(wtype)
		return !HCDW.lckmh_wtype(wtype, @actor_id)
	end
	
end

class Game_Actor < Game_Battler

	#==========================================================================
	# ! OVERWRITTEN METHOD
	#==========================================================================

	def release_unequippable_items(item_gain = true)
		loop do
			last_equips = equips.dup
			@equips.each_with_index do |item, i|
				if !equippable?(item.object, i) || item.object.etype_id != equip_slots[i]
					trade_item_with_party(nil, item.object) if item_gain
					item.object = nil
				end
			end
			return if equips == last_equips
		end
	end

end
