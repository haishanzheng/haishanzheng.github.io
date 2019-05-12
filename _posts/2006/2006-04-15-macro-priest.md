---
title:  "牧师宏"
date:   2006-04-15 11:28:49 +0800
---

牧师操作这么简单，还用宏干嘛。。。公布几个我用的宏。宏都存放在 游戏目录\WTF\Account\xxxxx\macro-cache.txt里，所以换机器时拷贝这个文件就好了。  

1. 给自己加盾宏：你可以选了你的头像再按盾，或者使用Alt+盾，但是这些都比不上只按一个键快。  

/script if (nil == UnitName("target")) then IsTargetNil = true; else IsTargetNil = false; end;   
/script TargetUnit("player"); CastSpellByName('真言术：盾(等级 10)');   
/script if (IsTargetNil) then ClearTarget(); else TargetUnit("playertarget"); end;   

简单解释：如果你当前没有目标，则给自己加盾并清除目标，否则给自己加盾并恢复目标。同样能量灌注也可用这个宏。注意用了宏后cd时间不会显示在宏上，你可以在旁边技能栏再放一个盾看cd。  

2. 整合攻击和治疗宏：牧师的特点是加血，偶尔扔扔痛和丢魔杖，所以可以根据目标的不同智能选择使用治疗还是攻击性魔法，节省按键。  

/script if (IsAltKeyDown()) then TargetUnit("player"); CastSpellByName('快速治疗(等级 7)'); TargetUnit("playertarget"); else if (UnitCanAttack("player", "target")) then CastSpellByName("射击"); else CastSpellByName("快速治疗(等级 7)"); end; end;  

简单解释：这个宏写的不完美，由于受宏字数255的限制（如果你加了插件另当别论），如果你开始没有选择任何目标，按alt给自己加血不会清除目标。不过这个情况比较少见。而且也不考虑被精神控制的情况。  

3. 喊话宏：没有魔法，被火焰之王沉默，要激活。  

/script chatMessage = "给我解魔法,我被沉默了,谢谢.....";  
/script if (GetNumRaidMembers() > 0) then chatType = "RAID"; elseif (GetNumPartyMembers() > 0) then chatType = "PARTY"; else chatType = "YELL"; end;  
/script SendChatMessage(chatMessage, chatType);  

简单解释：如果在团队中，就用团队频道讲话。没有团队看是否有小队，再没有就大喊。  

