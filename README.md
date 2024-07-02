# OfflineData
A set of minimized files for identifying locations in the general vicinity by type and name. These contain 25 different types of places worth visiting in a locative game, globally totalling about 7 million POIs. These files give you the ability to identify the name and type of place a player/user is in to roughly 14m(~45ft) without a connection to a separate server. For an example of these in use, see the WeekendSpaceProto repository here on GitHub or install the working client from https://cerol.itch.io/weekend-space-command-prototype

Information in the files was generated in March 2024, from planet.osm.pbf dated May 2023. 

# Usage
Download whichever set of files you want to be able to reference in your mobile app. (In the expected use case of being included in a Godot Engine game, unzipping the entire set of files will result in a multi-minute delay in startup times.)
The files can be used to draw images of a map tile as well as identify the names and terrain types of features are present in a PlusCode to approximately a Cell10 resolution. (In proper OpenLocationCode syntax, this would be 22334400+00. PraxisMapper removes trailing zeros when referring to PlusCodes for convenience).
```
{
  "olc":"8M3V46",
  "entries":{
    "suggestedmini":[
      {"c":"258,48","r":10,"tid":1},
      {"c":"113,75","r":9,"tid":1},
      {"c":"131,78","r":2,"nid":1,"tid":18},
      {"c":"258,51","r":2,"tid":19},
      {"c":"131,71","r":2,"nid":1,"tid":18}
    ]
  },
  "nameTable":{
    "1":"chin letters"
  }
}
```

Each file is a Cell6, and each 'pixel' in this is a Cell10, creating a 400x400 grid to reference. To determine a user's coordinates, start at 0,0 (lower left corner) in the current Cell6 of the players current PlusCode location. Add (index * 20) to both X and Y values matches the characters index in the Cell8 positions, and add index to both values for the characters in the Cell10 positions. EX: 8M3V4652+HM. Take the first 6 out (8M3V46) and load that file. Add 100 to Y for the 5 and 11 for H. Add 0 to X for the 2 and 13 for the M. Your player's location in this grid is 13, 111.

Schema:
* olc: Confirmation of which PlusCode Cell6 this data corresponds to. Each Cell6 file is 1 degree square on the map.
* entries: An array of dictionaries, each one corresponding to a single shape to be drawn by the client. In this minimized format, the only set will be "suggestedmini".
* * nid: Name ID. If present, indicates which entry in the nameTable is associated with this geometry item.
* * tid: Terrain ID. The MatchOrder property of a PraxisMapper style entry in the style set matching the entries name.
* * c: Center. The middle of this circle in the 400x400 grid this file represents.
* * r: Radius. How wide to draw the circle on the grid.
* nameTable: a dictionary of numbers and names. Key is nameId, Value is name. All names are unique in a file.

This data is sufficient to allow a client to draw 3 reasonably accurate images: The actual map tile using style data, a name map by attaching nid values to unique colors, and a terrain type map using terrain ids. The latter 2 images can be used by a client to reference a location by pixel and match the color back to a name or terrain type, though math can be done to determine distance to each point and check vs radius as well.

# License
Source data copyright OpenStreetMap Contributors. 
Processed files createdy by PraxisMapper. 
Licensed under the Open Data Commons Open Database License (ODbL). 
https://opendatacommons.org/licenses/odbl/
