# OfflineData
A set of files for identifying types of locations in the general vicinity. Generated with PraxisMapper.
These files give you the ability to identify when a player/user is within a roughly 300m block containing the selected area without a connection to a separate server.

#Usage
Download and unzip whichever set of files you want to be able to reference in your mobile app.
The files can be used to identify which features are present in a PlusCode to a Cell8 resolution. (In proper OpenLocationCode syntax, this would be 22334455+00. PraxisMapper removes trailing zeros when referring to PlusCodes for convenience).
Each zip contains up to 400 .json files, with this schema:

```
{
  "index": {
    "university,1|retail,2|tourism,3|wreck,4|historical,5|artsCulture,6|namedBuilding,7|water,8|wetland,9|park,10|beach,11|natureReserve,12|cemetery,13|trail,14" : {}
  },
  "22": {
    "33":{
      "44":{
        "55": "11",
        "3C": "11"
        }
      }
    }
  }
}
```

Each file covers a 20 degree x 20 degree area, corresponding to the first 2 digits of the PlusCode it represents. It then contains an 'index' dictionary, with the first entry's key containing the list of area types and their numerical id.
The index allows for multiple types of areas to be packed into one file, but generating those takes dramatically longer and you may only have an interest in one type of place.
Each dictionary contains another dictionary of child cells by character pairs, and this repeats for the first 6 characters. The 4th character pair, instead of containing another dictionary set, holds a pipe-delimited string of which index keys are present in that area.
In the example above, we can see that the PlusCodes 22334455 and 2233443C have a beach present. Excluded character pairs indicate there are no such areas inside the missing area.

#License
Source data copyright OpenStreetMap Contributors
Licensed under the Open Data Commons Open Database License (ODbL)
https://opendatacommons.org/licenses/odbl/
