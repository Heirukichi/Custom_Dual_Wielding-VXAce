# Custom Dual Wielding - VXAce
*Author: Heirukichi*



## **DESCRIPTION**
This script allows you to prevent actors from equpping certain weapon types in the dual wielding slot, even if the could normally wield them.

## **TABLE OF CONTENTS**
* [Installation](#installation)
* [Usage](#usage)
* [Terms of Use](#terms-of-use)

## **INSTALLATION**
Paste this script in your project below Materials. It should work fine even with other scripts that overwrite methods that are aliased in this one as long as you place this BELOW them.
However, keep in mind that this script overwrites the include? method of Window_EquipItem class. It is highly unlikely to work with any other script that overwrites that method and it should be placed ABOVE any other script aliasing that method in order to reduce compatibility issues.
Detailed instructions on how to configure this script can be found in the CONFIG module.



## **USAGE**
This script interacts with the following classes and methods. Methods are listed using the followinf rule.

Symbol | Meaning
-------|--------
\* | aliased method
\+ | new method
! | overwritten method

*Window_EquipItem*
- ! include?

*Game_Actor*
- ! release_unequippable_items

*Game_BattlerBase*
- \* equippable?
- \* equip_wtype_ok?
- \+ dual_wtype_allowed?
- \+ main_wtype_allowed?


## **TERMS OF USE**
This script is under the GNU General Public License v3.0. This means that:
- You are free to use this script in both commercial and non-commercial games as long as you give proper credits to me (Heirukichi) and provide a link to my website;
- You are free to modify this script as long as you do not pretend you wrote this and you distribute it under the same license as the original.

You can review the full license in the LICENSE file or check it [here](https://www.gnu.org/licenses/gpl-3.0.html)

In addition I'd like to keep track of games where my scripts are used so, even if this is not mandatory, I'd like you to inform me and send me a link when a game including my script is published.
As I said, this is not mandatory but it really helps me and it is much appreciated.

#### *IMPORTANT NOTICE*
If you want to distribute this code, feel free to do it, but provide a link to my website instead of pasting my script somewhere else.
