# Mega Man X Autosplitter for console (SD2SNES)

Created using v1.0 (JP) via Bizhawk, but should work on v1.1 (JP).  Has not been tested with English versions.

This was my first time doing something like this, so it was a fun project!  :)

Can be used with either [QUSB2SNES](https://skarsnik.github.io/QUsb2snes/) or [SNI](https://github.com/alttpo/sni) (Recommended).
You'll need to have the component for Livesplit installed [HERE](https://github.com/usb2snes/LiveSplit.USB2SNESSplitter)

json files don't allow comments, so here's a breakdown of what these do.

## Autostart
By default, this automatically starts your timer when the conditions match.  Alternatively if you prefer to start the timer manually, change "active" from 1 to 0.
```
{
  "name": "Mega Man X",
  "autostart": {
    "active": "1",
    "address": "0x02",       // This address is specifically where the cursor on the main menu is (Game Start, Password, Options)
    "value": "0xA6",         // Game Start is highlighted
    "type": "eq",
    "more": [                // "More" simply lets a split check multiple addresses simultaneously, and will execute when all conditions are met.
      {
        "address": "0x0A8",  // Address shows controller input
        "value": "0x10",     // Start button is pressed
        "type": "eq"
      }
    ]
  },
```

## Intro
Splits at the moment X is grabbed by Vile during the intro stage.
```
{
     "name": "Intro",
	 "address": "0x01F9B",    // The flag that determines if the Intro stage has been completed.  This is 0 on a new game and changes to 4 when completed.  Not really necessary but better safe than sorry.
	 "value": "0x0",          // Intro Stage is not completed.
	 "type": "eq",
     "more": [
	  {
	   "address": "0x0C32",   // Honestly, I have no clue what this address does.  Coltaho's autosplitter for emulator uses this address for this purpose so I used that.
	   "value": "0x30",       // Grab
       "type": "eq"
	  }
	  ]
	},
```

## Boss Doors (Excluding Storm Eagle)
Splits at the moment you touch the door before entering a boss room.

Storm Eagle in speedruns don't use the door, but X's checkpoint updates the moment you touch the rising platform, so the door check isn't needed.
```
     "name": "Chill Penguin Stage",
	 "address": "0x01F7A",	    // Address points to what stage X is at.
	 "value": "0x08",	    // Stage Value (In this case, 0x08 is Chill Penguin's stage)
	 "type": "eq",
     "more": [
	  {
	   "address": "0x01F81",    // Checkpoint on stage (Adding this check is to prevent Armor Part Capsules from splitting prematurely)
	   "value": "0x02",	    // Farthest checkpoint (Updates after entering the pre-boss room)
       "type": "eq"
	   },
	  {
	   "address": "0x01F",	    // This value consistently switches to 0x63 when doors are touched.
	   "value": "0x63",	    // X touches door.
	   "type": "eq"
	  }
	   ]
	},
```

## Mavericks and Sigma 1-3
Splits when helmet dings after completing a stage.  Works basically the same as boss door
```
{
     "name": "Chill Penguin Boss",
	 "address": "0x01F7A",
	 "value": "0x08",
	 "type": "eq",
     "more": [
	  {
	   "address": "0x01F81",    // Again, checkpoint check is used to avoid capsule from splitting.
	   "value": "0x02",
       "type": "eq"
	   },
	  {
	   "address": "0x0BBC",     // I think this is related to X's status or pose.
	   "value": "0xCB",         // The exact moment the helmet ding sound plays after sprite change, used when Armor Part is obtained or boss is defeated.
	   "type": "eq"
	  }
	   ]
	},
```

## Chill Penguin revisit for HP tank (100%)
Splits the moment the HP tank is obtained.
```
{
     "name": "Chill HP Grab",
	 "address": "0x01F9C",      // All HP tanks share a byte with each bit dedicated to each tank.  "0000 0001" in this byte belongs to Chill Penguin HP.
	 "value": "0x01",           // This is that bit.  (I assume I did this correctly.  It splits properly on pick up so not complaining)
	 "type": "bit"
	},
```

## Hadouken (100%)
Splits on helmet ding after getting Hadouken.

```
{
     "name": "Hadouken Obtain",
	 "address": "0x017FE",        // The value at this address increments by 1 each capsule area visit starting from 0x00 on a new playthrough.  Once you get Hadouken, this value becomes 0x85
	 "value": "0x85",
	 "type": "eq",
     "more": [
	  {
	   "address": "0x0BBC",       // Same as Bosses.  Helmet ding.
	   "value": "0xCB",
       "type": "eq"
	   }
	    ]
	},
```

## Wolf Sigma Defeat
This splits at the exact moment Wolf Sigma is defeated for the final split.
```
{
     "name": "Wolf Sigma Defeat",
	 "address": "0x01F7A",
	 "value": "0x0C",
	 "type": "eq",
     "more": [
	  {
	   "address": "0x0E69",  // Honestly, I have no idea what this address does, but it seems to be related to enemies.  While this works fine from testing, if anyone has a better address for this, let me know!
	   "value": "0x12",      // That address changes to this value at the exact moment Wolf Sigma is defeated, and only becomes this value at that point in the stage, which is why it works.
       "type": "eq"
	   }
	    ]
	}
```
